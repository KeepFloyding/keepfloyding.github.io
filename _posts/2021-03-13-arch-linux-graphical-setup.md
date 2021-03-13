---
layout: post
title: Setting up Linux Arch - Graphical Side
date: 2021-01-03 00:00:00 +0100
categories: [linux, arch, installation, xfce4, lightdm, plymouth]
tags: [linux, arch, installation, xfce4, lightdm, plymouth]
header-img: images/archbtw.png
---

This is a continuation of the previous blog post on setting up Linux Arch. Here we dive into the graphical environment and other tools necessary for the end-user to use this system as their daily driver.

## Graphical Environments

* Choose display manager and graphical environment. I have chosen xfce4 here with xorg due to simplicity but I do like using Wayland so will be looking to add an entry on setting this up with Wayland:
  ```
  sudo pacman -S xorg xorg-server mesa
  sudo pacman -S xfce4 xfce4-goodies
  ```

* Configure your xfce4 desktop environment. Things that should be configure are the panel, terminal and keyboard shortcuts.

  Files can be found here: insert Github link. Terminal is editted with the `terminalrc` file in **.config/xfce4/terminal/**. All other files are found in **.config/xfce4/xfconf/xfce-perchannel-xml/**.

* Configure your xfce4 windows manager. Here mostly I just customize shortcuts for window manager switching. Configuration can be found here:

  ...

* Install and configure a display manager (I like lightdm):
  ```
  sudo pacman -S lightdm lightdm-gtk-greeter
  systemctl enable lightdm
  ```

  Further configuration can be done by modifying the file here: **/etc/lightdm/lightdm.conf**.

* Setup Plymouth for flicker free boot process (https://wiki.archlinux.org/index.php/plymouth). You will need to add the Plymouth hooks to your initramfs. There are some cool designs here that you can use: https://github.com/adi1090x/plymouth-themes   

    ```
    sudo pacman -S plymouth
    sudo systemctl enable lightdm-plymouth
    plymouth-set-default spinner
    ```

## Audio and Bluetooth

* Configure Audio. Using pulse-audio and pavu control as the front-end. XFCE4 also has a plugin for pulse audio that can be installed.

  ```
  sudo pacman -S pulseaudio pavu-control
  ```

* Configure bluetooth with `bluez` and `bluez-utils`. Bluetooth can then be configured with `bluetoothctl`. If you want a graphical frontend then `blueman` is not too bad.

  ```
  sudo pacman -S bluez bluez-utils blueman
  ```

## Additional Tools

* Install a final few other things that will come in useful.

  ```
  sudo pacman -S git openssh redshift code opera xcape rofi xclip file-roller conky base-devel
  ```

* Install a package manager that can make use of the AUR (Arch User Repository). Installation for `yay` (yet another yaourt) can be found [here](https://github.com/Jguer/yay).

  ```
  git clone https://aur.archlinux.org/yay.git
  cd yay
  makepkg -si
  ```

* Configure rofi to be your application launcher as detailed in my blog post here.

## Reboot

* Reboot into the system and you should be taken to the login manager. Login to the system.
