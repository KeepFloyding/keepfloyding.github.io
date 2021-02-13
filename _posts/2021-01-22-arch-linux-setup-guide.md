---
layout: post
title: Rapid Arch Linux Setup
date: 2021-01-03 00:00:00 +0100
categories: [linux, arch, installation, btrfs]
tags: [linux, arch, installation, btrfs]
header-img: images/arch_btw.png
seo:
  date_modified: 2021-01-19 17:04:31 +0000
---

Setting up Arch Linux is actually surprisingly easy due in great part to the fantastic Arch [Wiki](https://wiki.archlinux.org/index.php/installation_guide). However, since there are so many one-time commands that need to be executed before you have a workable distribution, it is easy to forget how you set it up in the first place. In this blog post, I outline a quick and simplified set up for a minimal Arch Linux install that has an encrypted root and uses the BTRFS file system. I'll cover the set up of the graphical environment in a later blog post.

# Setup Guide

1. Download the Arch Linux iso from [here](https://archlinux.org/download/) and write it to a usb. It is as simple as using `cp` from the command line:

    ```
    cp path/to/archlinux.iso /dev/sdx
    ```

    You can find out what the device name is by using `lsblk` to list your device tree.

2. Boot into the installation environment on your desired machine. This can be done by accessing your hardware BIOS so that you boot from usb.

3. Change to your keyboard layout. In British English, it would be:

    ```
    loadkeys uk.map.gz
    ```

4. Do your disk partitioning with `fdisk`. Normally you would want the following 3 separate partitions: root (`\`), `boot` and `swap`. If you don't feel like it, you can just skip having the swap partition (read [this](https://www.lifewire.com/do-you-need-swap-partition-2202049) if you are not sure).

    The list of commands for your boot partition (assuming you want EFI with around half a GB of memory) is simply:
    ```
    fdisk /dev/sdx
    n
    p
    1
    [ENTER]
    +512M
    t
    L
    ef (or whatever the EFI entry is in the list)
    ```

    `n` refers to creating a new partition, `[ENTER]` refers to accepting the default suggestion by pressing the `Enter` key and `t` refers to transforming your partition type. To create your root partition (assuming you are still in the `fdisk` dialogue).

    ```
    n
    p
    2
    [ENTER]
    [ENTER]
    w
    ```

    The command above just assigns the rest of the space in your disk to root. The `w` at the end writes your changes.

5. Disk encryption time! I would advise every computer user to encrypt their drive just so that your information isn't compromised in case your computer gets stolen (it's ridiculously easy to read information from another hard drive that is not encrypted). Here I go for a simple LUKS (LUKS stands for **Linux Unified Key Setup**) encryption directly on your root partition. The commands are:

    ```
    cryptsetup -y -v luksFormat /dev/sda2
    cryptsetup open /dev/sda2 cryptroot
    ```

    You can replace `cryptroot` with another name if you so wish.

6. Creating and  mounting your file system. In this case, we will be using BTRFS for root and FAT for EFI.

    ```
    mkfs.btrfs /dev/mapper/cryptroot
    mkfs.fat -F32 /dev/sda1
    mount -t btrfs /dev/mapper/cryptroot /mnt
    ```

7. BTRFS configuration. Set the following parameters:

    ```
    o_btrfs=defaults,x-mount.mkdir,compress=lzo,ssd,noatime
    ```

    Now we can create the BTRFS subvolumes. In this case, we will create separate ones for root and home. We will also create another one where we can store our snapshots.

    ```
    btrfs subvolume create /mnt/root
    btrfs subvolume create /mnt/home
    btrfs subvolume create /mnt/snapshots
    ```

    Then unmount the main system, so that the subvolumes can be mounted instead.

    ```
    umount -R /mnt
    mount -t btrfs -o subvol=root,$o_btrfs /dev/mapper/cryptroot /mnt
    mount -t btrfs -o subvol=home,$o_btrfs /dev/mapper/cryptroot /mnt/home
    mount -t btrfs -o subvol=snapshots,$o_btrfs /dev/mapper/cryptroot /mnt/.snapshots
    ```

    Create the boot directory and mount it.

    ```
    mkdir /mnt/boot
    mount /dev/sda1 /mnt/boot
    ```

8. Infer your local time from your internet connection using ntp.

    `timedatectl set-ntp true`

9. Install the base Arch system with the `pacstrap` command (here just installing the base kernel):

    ```
    pacstrap /mnt base linux linux-firmware btrfs-progs nano
    ```

10. Generate the filesystem table with `genfstab`. This automatically generates the config file that specifies which devices should be mounted on boot (in **/mnt/etc/fstab**.

    ```
    genfstab -U /mnt >> /mnt/etc/fstab
    ```

11. Enter your arch system as the root user. Now that we have the arch system installed, we can enter it with the `arch-chroot` command:

    ```
    arch-chroot /mnt
    ```

12. Change your hostname to something more interesting:

    ```
    echo awesome_host >> /etc/hostname
    ```

    You would need to configure the **/etc/hosts** so that you can identify the local host

    ```
    nano /etc/hosts
    ```
    Should be something like this

    ```
     127.0.0.1	localhost
    ::1		localhost
    127.0.1.1	awesome_host
    ```   

13. Configure your internet connect by enabling the systemd network modules.

    ```
    systemctl enable systemd-resolved   
    systemctl enable systemd-networkd

    ```

    Also you need to create these files for wired and wireless connections (replace the name with the the actual name of your network interface device that can be found with `ip link show`).

    ```
    /etc/systemd/network/20-wired.network
    [Match]
    Name=enp1s0

    [Network]
    DHCP=yes
    ```

    For a wireless network, you would create the one below.

    ```
     /etc/systemd/network/25-wireless.network
    [Match]
    Name=wlp2s0

    [Network]
    DHCP=yes
    ```

14. Redo `mkinitcpio` since we are encrypting our system and using btrfs. First you have to edit the file **/etc/mkinitcpio.conf** and update the hooks to include `btrfs` and `encrypt`. You should have something as given below:

    ```
    HOOKS=(base udev autodetect keyboard consolefgeont modconf block encrypt btrfs filesystems fsck)
    ```

    Redo `mkinitcpio` with the following command:

    ```
    mkinitcpio -P
    ```

15. Set a password for root with `passwd`


15. Install GRUB as the bootloader.

    ```
    pacman -S grub efibootmgr
    grub-install --target=x86_64-efi --efi-directory=/boot --bootloader-id=GRUB
    ```

16. Edit the grub default script to configure the correct kernel parameters that are passed by the bootloader in **/etc/default/grub**. You should edit the `GRUB_CMDLINE_LINUX` entry.

    ```
    GRUB_CMDLINE_LINUX="cryptdevice=/dev/sda2:cryptroot root=/dev/mapper/cryptroot"
    ```

    Then generate the GRUB config file with the following command:

    ```
    grub-mkconfig -o /boot/grub/grub.cfg
    ```

17. Create a user so that you don't have to log in as root all the time:

    ```
    pacman -S sudo vi
    useradd -m username
    passwd username
    usermod --append --groups wheel example_user
    visudo
    ```

    Uncomment the line to allow members of group wheel to have sudo privleges.

    ```
    %wheel ALL=(ALL) ALL
    ```

18. You should now be able to `reboot` into your system where you will first be asked to decrypt the hard drive before logging in as the user that you created above.


19. Now you can configure Arch Linux to your liking. This is a bare-bones install; if you would like to configure a graphical environment as well as other utilities then follow along to the next blog post where I will show how to this with Xorg and Wayland.
