---
layout: post
title: Rapid Arch Linux Setup 2 
date: 2021-03-15 00:00:00 +0100
categories: [linux, arch, installation, xfce4]
tags: [linux, arch, installation, xfce4]
header-img: images/archbtw.png
---

The previous Arch Linux post looked at setting up a bare-bones headless system with BTRFS and an encrypted drive. This post will look at setting up the front-end and making your system more suitable as a daily driver. There are several desktop environments that can be chosen, but here I have focused on simplicity and minimalism so it should be good for older systems. 

# Setting up the GUI


## Desktop Environment Installation and Customisation

![Alt Text](https://keepfloyding.github.io/images/arch_linux_2.png)


* For the desktop manager, I really like the simplicity behind XFCE4. It is customisable, easy to configure, lightweight and powerful. You start by simply installing it (along with some goodies).

    ```
    sudo pacman -S xfce4 xfce4-goodies
    ```
    
* The default view is not so attractive but this can be easily changed. You can easily change the theming of the window manager by downloading some cool themes from here: https://www.xfce-look.org/p/1099856. Download for instance the `Dracula` tar file and extract it in the `.themes` folder of your home directory. You can them choose this theme in the `Style` tab of the `Appearance` settings manager.


* You can do this also for icons and fonts. 

* For the panel configuration, I like to remove the 2nd panel and adjust the 1st one to have the following:
    * Show only icons for the windows to save on space
    * Change the menu to only show the mouse xfce4 icon
    * Configure the panel size
The settings will be shared in the Git repo. 

* And of course everyone's favourite customisation, changing to a cool background. I like the minimalistic ones like here: https://icanbecreative.com/article/simple-desktop-wallpapers-for-minimalist-lovers/ 

* Keyboard shortcuts! The shortcuts that I like to use are:
    * Fill window to the left or right (Super + Right Arrow (or Left Arrow))
    * Maximise window (Super + Up Arrow)
    * Launch file manager (Super + e)
    * Launch terminal (Ctrl + Alt + t)


## Display Manager Installation and Customisation

* For the Display Manager, I like to use LightDM (https://wiki.archlinux.org/title/LightDM). You also need to install a greeter to enable user login. I also install the `lightdm-gtk-greeter-settings` to easily configure the greeter with a GUI. 

    ```
    sudo pacman -S lightdm lightdm-gtk-greeter lightdm-gtk-greeter lightdm-gtk-greeter-settings
    ```

* Any customisation can be done with the `lightdm-gtk-greeter-settings`.

## App Launcher 

* For an efficient workflow, an app launcher that can be used to search and quick launch a desired application is crucial. I wrote a previous post about using Rofi which is absolutely excellent (https://keepfloyding.github.io/posts/rofi-config-on-linux/). However, I am trying out a new app launcher called `Ulauncher` which looks very slick (https://ulauncher.io/). To set it up:

    ```
    git clone https://aur.archlinux.org/ulauncher.git
    cd ulauncher
    makepkg -is 
    ```

* Any hot key can be configured to launch Ulauncher. I typically like using the Super key which is used for other shortcuts as well. In a similar manner to a Rofi setup, you can use `xcape` to set a single use press for the Super key using something like:
    ```
    xcape -e 'Super_L=Shift_L|Control_L|Super_L'
    ```
    Then you can configure the hot key above to be used to launch Ulauncher. Just make sure to add the line above to `~/.xprofile` so that `xcape` is configured everytime you log on.


## YAY and other tools

* Some other important tools that I find useful should be installed such as:

    ```
    sudo pacman -S git openssh redshift code firefox xclip file-roller gedit base-devel
    ```

* YAY (Yet Another Yaourt) is a package manager useful for install packages from the AUR. This can be configured with 

    ```
    git clone https://aur.archlinux.org/yay.git
    cd yay
    makepkg -si
    ``` 

## Sound and Bluetooth

* Configure Audio. Using pulse-audio and pavu control as the front-end. XFCE4 also has a plugin for pulse audio that can be installed.

    ```
    sudo pacman -S pulseaudio pavucontrol
    ```

* Configure bluetooth with bluez and bluez-utils. Bluetooth can then be configured with `bluetoothctl`.

    ```
    sudo pacman -S bluez bluez-utils
    sudo systemctl enable bluetooths
    ```

## Timeshift (or Snapper)




