Arch Linux Setup Guide:

* Grab Arch iso and boot from it

* ip link to check the wifi hardware; ip link show

* Configure wifi with iwctl
iwctl -> follow it

* timedatectl set-ntp true

* Partition the disks with fdisk
    -> swap and root  partitions, and boot?
fdisk hardrive of choice
d to delete existing partitikons
n to create new ones

EFI -> n, Y, +512M, t, EFI
Root -> n, Y, Y

How do I encrypt the disk? -> LUKS on a partition

* prepare the disk pre-encryption
* create the normal partitions
# cryptsetup -y -v luksFormat /dev/sda2
# cryptsetup open /dev/sda2 cryptroot
# mkfs.ext4 /dev/mapper/cryptroot
# mount /dev/mapper/cryptroot /mnt

mkfs.fat -F32 /dev/sda1 -> EFI partition system

* configure mkinitcpio
HOOKS=(base udev autodetect keyboard keymap consolefont modconf block encrypt filesystems fsck)
* configure the boot loader
cryptdevice=UUID=device-UUID:cryptroot root=/dev/mapper/cryptroot

and then that should give you your encrypted root :D



* Format the file systems as needed; 
    -> choose btrfs (how do we do this?)
    -> mkswap

[how do you setup btrfs...]

* Mount partitions
    -> root is mounted on /mnt 
    -> swapon the swap partition

* Edit the mirrorlist to find closest mirrors 

* pacstrap /mnt base linux linux-firmware

* genfstab -U /mnt >> /mnt/etc/fstab

* arch-chroot /mnt

* Network

CREATE /etc/hostname with hostname


systemctl enable systemd-resolved
systemctl enable systemd-networkd
 Create this file: 

 /etc/systemd/network/25-wireless.network
[Match]
Name=wlp2s0

[Network]
DHCP=yes

* mkinitcpio -P

* bootloader time -> most likely GRUB 

sudo pacman -S GRUB 
grub-install --target=x86_64-efi --efi-directory=esp --bootloader-id=GRUB
grub-mkconfig -o /boot/grub/grub.cfg
Make sure to detect other operating systems 

* Reboot and login as root

* Create user so that you don't mess around as root

* Install a few other things:
 * git
* openssh
* redshift
* code (VS code editor)
* nano
* vim
* firefox
* xcape
* rofi
* xfce4 (all the packages)
* xclip
* Xorg (I know maybe should change to Wayland)
* linux-firmware
* file-roller
* gedit
* conky

* Display Manager yo

* Choose Xfce Session from the menu in a display manager of choice, or add exec startxfce4 to Xinitrc.
