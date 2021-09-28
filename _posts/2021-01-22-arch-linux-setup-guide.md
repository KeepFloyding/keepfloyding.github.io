---
layout: post
title: Rapid Arch Linux Setup
date: 2021-01-03 00:00:00 +0100
categories: [linux, arch, installation, btrfs]
tags: [linux, arch, installation, btrfs]
header-img: images/archbtw.png
seo:
  date_modified: 2021-03-21 14:45:18 +0000
---

Setting up Arch Linux is actually surprisingly easy due in great part to the fantastic Arch [Wiki](https://wiki.archlinux.org/index.php/installation_guide). However, since there are so many one-time commands that need to be executed before you have a workable distribution, it is easy to forget how you set it up in the first place. In this blog post, I outline a quick and simplified set up for a minimal Arch Linux install that has an encrypted root and uses the BTRFS file system. I'll cover the set up of the graphical environment in a later blog post.

# Setup Guide

## Booting into Installation

* Download the Arch Linux iso from [here](https://archlinux.org/download/) and write it to a usb. It is as simple as using `cp` from the command line:

    ```
    cp path/to/archlinux.iso /dev/sdx
    ```

    You can find out what the device name is by using `lsblk` to list your device tree.

* Boot into the installation environment on your desired machine. This can be done by accessing your hardware BIOS so that you boot from usb.

*  Change to your keyboard layout. In British English, it would be:

    ```
    loadkeys uk.map.gz
    ```

## Disk Partition and Encryption


* Do your disk partitioning with `fdisk`. Normally you would want the following 3 separate partitions: root (`\`), `boot` and `swap`. If you don't feel like it, you can just skip having the swap partition (read [this](https://www.lifewire.com/do-you-need-swap-partition-2202049) if you are not sure).

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

    `n` refers to creating a new partition, `[ENTER]` refers to accepting the default suggestion by pressing the `Enter` key and `t` refers to transforming your partition type.

    Next up we create the swap partition with 8GB memory (assuming you are still in the `fdisk` dialogue).

    ```
    n
    p
    2
    [ENTER]
    +8000M
    t
    2
    82 (or whatever the entry is for swap)
    ```

    To create your root partition:

    ```
    n
    p
    3
    [ENTER]
    [ENTER]
    w
    ```

    The command above just assigns the rest of the space in your disk to root. The `w` at the end writes your changes to the hardrive and exits the `fdisk` dialogue.

* Disk encryption time! I would advise every computer user to encrypt their drive just so that your information isn't compromised in case your computer gets stolen (it's ridiculously easy to read information from another hard drive that is not encrypted). Here I go for a simple LUKS (LUKS stands for **Linux Unified Key Setup**) encryption directly on your root partition. The commands are:

    ```
    cryptsetup -y -v luksFormat /dev/sda3
    cryptsetup open /dev/sda3 cryptroot
    ```

    You can replace `cryptroot` with another name if you so wish.

* To create your encrypted swap, you do the following:

    ```
    cryptsetup open --type plain --key-file /dev/urandom /dev/sda2 cryptswap
    mkswap -L swap /dev/mapper/cryptswap
    swapon /dev/mapper/cryptswap
    ```

## Creating and Mounting BTRFS

* Creating and  mounting your file system. In this case, we will be using BTRFS for root and FAT for EFI.

    ```
    mkfs.btrfs /dev/mapper/cryptroot
    mkfs.fat -F32 /dev/sda1
    mount -t btrfs /dev/mapper/cryptroot /mnt
    ```

* BTRFS configuration. Set the following parameters:

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

## Installation and Configuration

* Install the base Arch system with the `pacstrap` command (here just installing the base kernel):

    ```
    pacstrap /mnt base linux linux-firmware btrfs-progs nano
    ```

* Generate the filesystem table with `genfstab`. This automatically generates the config file that specifies which devices should be mounted on boot (in **/mnt/etc/fstab**).

    ```
    genfstab -U /mnt >> /mnt/etc/fstab
    ```

* Enter your arch system as the root user. Now that we have the arch system installed, we can enter it with the `arch-chroot` command:

    ```
    arch-chroot /mnt
    ```

* Redo `mkinitcpio` since we are encrypting our system and using BTRFS. First you have to edit the file **/etc/mkinitcpio.conf** and update the hooks to include `btrfs` and `encrypt`. You should have something like below:

    ```
    HOOKS=(base udev autodetect keyboard consolefgeont modconf block encrypt btrfs filesystems fsck)
    ```

    Redo `mkinitcpio` with the following command:

    ```
    mkinitcpio -P
    ```

* Set a password for root with `passwd`

* Create a user so that you don't have to log in as root all the time:

    ```
    pacman -S sudo
    useradd -m example_user
    passwd example_user
    usermod --append --groups wheel example_user
    nano /etc/sudoers
    ```

    Uncomment the line to allow members of group wheel to have sudo privleges.

    ```
    %wheel ALL=(ALL) ALL
    ```

## Internet Connection

* Infer your local time from your internet connection using ntp.

    `timedatectl set-ntp true`


* Change your hostname to something more interesting:

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

* Configure your internet connect by enabling the systemd network modules.

    ```
    systemctl enable systemd-resolved   
    systemctl enable systemd-networkd
    ```

    Also you need to create these files for wired and wireless connections (replace the name with the the actual name of your network interface device; this can be found with `ip link show`).

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

## Modifications for Encrypted Swap

  * You will need to uncomment a line in  **/etc/crypttab** related to the swap partition. You should have something like below:

      ```
      # <name>  <device>     <password>     <options>
      swap      /dev/sda2    /dev/urandom   swap,cipher=aes-xts-plain64,size=256
      ```
  * The UUID will always change for the /dev/mapper/swap partition; so you will need to make sure that it is mounted by its label rather than its UUID. In this case, just modify the **/etc/fstab** so that the swap partition is mounted by its label:

      ```
      # <filesystem>    <dir>  <type>  <options>  <dump>  <pass>
      LABEL=swap  none   swap    defaults   0       0t
      ```

## Bootloader

* Install GRUB as the bootloader.

    ```
    pacman -S grub efibootmgr
    grub-install --target=x86_64-efi --efi-directory=/boot --bootloader-id=GRUB
    ```

* Edit the GRUB default script to configure the correct kernel parameters that are passed by the bootloader in **/etc/default/grub**. You can do this by editing the `GRUB_CMDLINE_LINUX` entry.

    ```
    GRUB_CMDLINE_LINUX="cryptdevice=/dev/sda3:cryptroot root=/dev/mapper/cryptroot"
    ```

    Then generate the GRUB config file with the following command:

    ```
    grub-mkconfig -o /boot/grub/grub.cfg
    ```



## Conclusion and Next Steps

* You should now be able to `reboot` into your system where you will first be asked to decrypt the hard drive before logging in as the user that you created above.


* Now you can configure Arch Linux to your liking. This is a bare-bones install; if you would like to configure a graphical environment as well as other utilities then follow along to the next blog post where I will show how to this with Xorg and Wayland.

## References

I've listed some references that I found super useful in configuring this guide:

* [Bullet Proof Arch Install](https://wiki.archlinux.org/index.php/User:Altercation/Bullet_Proof_Arch_Install)
* [Wiki Install](https://wiki.archlinux.org/index.php/installation_guide)
* [FOSS Install](https://itsfoss.com/install-arch-linux/)
* [BTRFS Setup](https://github.com/egara/arch-btrfs-installation)
* [Bespinian Install - minimal guide to installing Arch with encryption and Walyand](https://blog.bespinian.io/posts/installing-arch-linux-on-uefi-with-full-disk-encryption/)
