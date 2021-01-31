---
layout: post
title: Fast Arch Linux Setup
date: 2021-01-03 00:00:00 +0100
categories: [linux, arch, installation, xfce4]
tags: [linux, arch, installation, xfce4]
header-img: images/arch_btw.png
seo:
  date_modified: 2021-01-19 17:04:31 +0000
---

Setting up Arch Linux is actually surprisingly easy due in great part to the fantastic Arch [Wiki](insert_wiki_link). However, since there are so many one-time commands that need to be executed before you have a workable distribution, it is easy to forget how you set it up in the first place. In this blog post, I outline a quick and simplified setup for Arch Linux all the way up to the xfce graphical environment. 

# Setup Guide

1. Download the Arch Linux iso from [here](insert_download_link) and write it to a usb. It is as simple as using `cp` from the command line:

```
cp path/to/archlinux.iso /dev/sdx
```

You can find out what the device name is by using `lsblk` to list your device tree. 

2. Boot into installation environment on your desired machine. 

3. Change to your keyboard layout. In British English, it would be:

```
loadkeys uk.map.gz
```

4. Do your disk partitioning with `fdisk`. Normally you would want the following 3 seperate partitions: root (`\`), `boot` and `swap`. If you don't feel like it, you can just skip having the swap partition (read [this](https://www.lifewire.com/do-you-need-swap-partition-2202049) if you are not sure).

The list of commands for your boot partition (assuming you want EFI with around half a GB of memory) is simply:
```
fdisk /dev/sdx
n
p 
1
Y
+512M
t
L
ef (or whatever the EFI entry is in the list)
```. 
`n` refers to creating a new partition, `Y` refers to accepting the default suggestion and `t` refers to transforming your partition type. To create your root partition (assuming you are still in the `fdisk` dialogue).
```
n
p
2
Y
Y
w
```

The command above just assigns the rest of the space in your disk to root. 

5. Disk encryption time! I would advise every computer user to encrypt their drive just so that your information isn't compromised in case your computer gets stolen (it's ridiculously easy to read information from another hard drive that is not encryted). I go for the simplest setting which is simple LUKS encryption directly on your partition. Fun fact LUKS stands for **Linux Unified Key Setup**. The commands are:

```
cryptsetup -y -v luksFormat /dev/sda2
cryptsetup open /dev/sda2 cryptroot
```

6. Creating and  mounting your file system. In this case, we will be using btrfs for root and FAT for EFI. 

```
mkfs.btrfs /dev/mapper/cryptroot
mkfs.fat -F32 /dev/sda1 
mount -t btrfs /dev/mapper/cryptroot /mnt
mkdir /mnt/boot
mount /dev/sda1 /mnt/boot
``` 

7. BTRFS configuration. Set parameters to avoid repetition

```
o_btrfs=defaults,x-mount.mkdir,compress=lzo,ssd,noatime
```

Creation of btrfs subvolumes is next

```
btrfs subvolume create /mnt/root
btrfs subvolume create /mnt/home
btrfs subvolume create /mnt/snapshots
```

Then unmount the main system, so that the subvolumes can be mounted instead. 

```
umount -R /mnt
mount -t btrs -o subvol=root,$o_btrfs /dev/mapper/cryptroot /mnt
mount -t btrs -o subvol=home,$o_btrfs /dev/mapper/cryptroot /mnt/home
mount -t btrs -o subvol=snapshots,$o_btrfs /dev/mapper/cryptroot /mnt/.snapshots
```

7. Infer your local time from your internet connection using ntp. 

`timedatectl set-ntp true`

8. Install the base Arch system with the `pacstrap` command (here just installing the base kernel):

```
pacstrap /mnt base linux linux-firmware btrfs-progs nano
```

9. Generate the filesystem table with `genfstab`. This automatically generates the config file that specifies which devices should be mounted (in **/mnt/etc/fstab**.

```
genfstab -U /mnt >> /mnt/etc/fstab
```

 10. Enter your arch system as the root user. Now that we have the arch system installed, we can enter it with the `arch-chroot` command:

 ```
 arch-chroot /mnt
 ```

 11. Change your hostname to something more interesting than **archiso**. 

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

 12. (Optional) Configure your WiFi. Easy way to configure WiFi is to use the default tools in `systemd`. 

```
systemctl enable systemd-resolved
systemctl enable systemd-networkd

```

Also you need to create this file (replace the name with the WiFi interface device (can be found `ip link show`))

```
 /etc/systemd/network/25-wireless.network
[Match]
Name=wlp2s0

[Network]
DHCP=yes
```

13. Redo mkinitcpio since we are encrypting our system and using btrfs. **/etc/mkinitcpio.conf**

```
HOOKS=(base udev autodetect keyboard consolefgeont modconf block encrypt btrfs filesystems fsck)
mkinitcpio -P
```

14. Set a password for root with `passwd`


15. Installing GRUB as a bootloader. 

```
pacman -S grub efibootmgr
grub-install --target=x86_64-efi --efi-directory=/boot --bootloader-id=GRUB
```

16. Edit the grub default script to pass the correct kernel parameters that are passed by the bootloader **/etc/default/grub**

```
GRUB_CMDLINE_LINUX="cryptdevice=/dev/sda2:cryptroot root=/dev/mapper/cryptroot"
```

```
grub-mkconfig -o /boot/grub/grub.cfg
```


17. Install a few other things

```
sudo pacman -S git openssh redshift code nano vim firefox xcape rofi xfce4 xclip xorg linux-firmware file-roller gedit conky lightdm
```

18. Reboot to check if your system can reboot without the iso with `reboot` after using `exit` to quit `chroot`.


19. Create user so that you don't mess around as root

20. Choose Xfce Session from the menu in a display manager of choice, or add exec startxfce4 to Xinitrc.

21. Configure Audio. Using pulse-audio and pavu control as the front-end. XFCE4 also has a plugin for pulse audio that can be installed.

```
sudo pacman -S pulseaudio pavu-control
```

22. Configure bluetooth with bluez and bluez-utils. Bluetooth can then be configured with `bluetoothctl`. 


