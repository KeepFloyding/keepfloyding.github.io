---
layout: post
title: Arch Linux GUI Setup 
date: 2021-03-15 00:00:00 +0100
categories: [linux, arch, installation, xfce4, ulauncher, snapper, btrfs, grub]
tags: [linux, arch, installation, xfce4, ulauncher, snapper, btrfs, grub]
header-img: images/archbtw.png
---

The previous Arch Linux [post](https://keepfloyding.github.io/posts/arch-linux-setup-guide/) looked at setting up a bare-bones headless system with BTRFS and an encrypted drive. This post will look at setting up the front-end and making your system more suitable as a daily driver. There are several desktop environments that can be chosen, but here I have focused on simplicity and minimalism so it should be suitable for older systems. 

# Setting up the GUI


## Desktop Environment Installation and Customisation

* For the desktop manager, I really like the simplicity behind [Xfce4](https://www.xfce.org/). It is customisable, easy to configure, lightweight and powerful (even though some of the folks at [Late Night Linux](https://latenightlinux.com/) make fun of it for being overly simplistic). I install it along with some goodies for a more full desktop experience. 

    ```
    sudo pacman -S xfce4 xfce4-goodies
    ```
    

![Alt Text](https://keepfloyding.github.io/images/arch_linux_2.png)



* The default view is not so attractive but this can be easily changed. You can download some cool themes from [here](https://www.xfce-look.org/p/1099856). For instance you can download the `Dracula` tar file and extract it in the `.themes` folder of your home directory. You can then choose this theme in the `Style` tab of the `Appearance` settings manager.


* You can do this also for icons and fonts from the same website. However, I am pretty happy with the defaults.  

* For the panel configuration, I like to remove the 2nd panel and adjust the 1st one to have the following:
    * Show only icons for the windows to save on space
    * Change the menu to only show the xfce4 icon
    * Configure the panel size to be larger.

I'll share the config files for this [here](https://github.com/KeepFloyding/keepfloyding.github.io/tree/master/_posts/code/xfce4.tar.gz)

* And of course everyone's favourite customisation, changing to a cool background. I like the minimalistic ones found [here](https://icanbecreative.com/article/simple-desktop-wallpapers-for-minimalist-lovers/). 

* Keyboard shortcuts! The shortcuts that I like to use are:
    * Fill window to the left or right `(Super + Right Arrow (or Left Arrow))`
    * Maximise window `(Super + Up Arrow)`
    * Launch file manager `(Super + e)`
    * Launch terminal `(Ctrl + Alt + t)`
    * Move window to left/right workspace `(Ctrl + Shift + Alt + Right Arrow (or Left Arrow))`


## Display Manager Installation and Customisation

* For the Display Manager, I use LightDM (https://wiki.archlinux.org/title/LightDM). You also need to install a greeter to enable user login. I also install the `lightdm-gtk-greeter-settings` to easily configure the greeter with a GUI. 

    ```
    sudo pacman -S lightdm lightdm-gtk-greeter lightdm-gtk-greeter-settings
    ```

* Any customisation can be done with `lightdm-gtk-greeter-settings`.

## App Launcher 

* For an efficient workflow, an app launcher that can be used to search and quick launchly a desired application is crucial. I wrote a previous [post](https://keepfloyding.github.io/posts/rofi-config-on-linux/) about using Rofi which is an absolutely excellent application. However, I am trying out a new app launcher called [`Ulauncher`](https://ulauncher.io/) which looks very slick. To set it up:

    ```
    git clone https://aur.archlinux.org/ulauncher.git
    cd ulauncher
    makepkg -is 
    ```

* Any hot key can be configured to launch Ulauncher. I typically like using the `Super` key which is used as a modifier key for other shortcuts as well. In a similar manner to a Rofi setup, you can use `xcape` to set a single press use for the Super key using something like:
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

* Redshift to make the screen easier on the eyes. I don't bother with the geolocation and just set it to have a permanent redshift by adding the following to `.xprofile`

    ```
    redshift -P -O 3500
    ```


## Sound and Bluetooth

* Configure Audio. Using pulse-audio and pavu control as the front-end. Xfce4 also has a plugin for pulse audio that can be installed.

    ```
    sudo pacman -S pulseaudio pavucontrol
    ```

* Configure bluetooth with bluez and bluez-utils. You also need to install the `pulseaudio-bluetooth` package to enable audio to work over bluetooth. Bluetooth can then be configured with `bluetoothctl`.

    ```
    sudo pacman -S bluez bluez-utils pulseaudio-bluetooth
    sudo systemctl enable bluetooth
    ```
    
* Pipewire is the latest and greatest multimedia framework in Linux. If you want to give it a try for audio, you can install `pipewire-pulse` which replaces the `pulseaudio` and `pulseaudio-bluetooth` packages. 

    ```
    sudo pacman -S pipewire-pulse
    systemctl start --user pipewire-pulse.service
    ```
    
    You should be able to see what is being used with `pactl info`. The `Server Name` should specify that it is running on `Pipewire`. 

## Timeshift (or Snapper)

* One of the good things about using BTRFS is that you can do automatic snapshots with either Snapper or Timeshift. I prefer Timeshift but had trouble building it, so will try with snapper. 

    ```
    sudo pacman -S snapper
    ```

* To create a config for automatic snapshots of your root system, you can do
    ```
     snapper -c root create-config /
    ```

* To create a custom snapshot:
    ```
    snapper -c root create --description test_snapshot
    ```

* All configs are available in `/etc/snapper/configs/`. They can be modified to adjust the snapshot frequency. 

* If you don't have a cron daemon running already, then you can use their own systemd units:

    ```
    systemctl enable snapper-timeline.timer
    systemctl enable snapper-cleanup.timer
    ```


## Grub Configuration

* In order to detect different operating systems you might have on your device, you will need `os-prober`. Then you need to mount these drives onto your system before reconfiguring grub.
    ```
    sudo pacman -S os-prober
    sudo mount /dev/sdx /mnt
    grub-mkconfig -o /boot/grub/grub.cfg
    ```

    You may need to add the following entry to /etc/default/grub

    ```
    GRUB_DISABLE_OS_PROBER=false
    ```

* To make changes to grub, I like using the front-end `grub-customizer`
    ```
    sudo pacman -S grub-customizer
    ```

* Also I remove the `quiet` argument from the kernel parameters in GRUB so that I can see the messages during bootup. 

* Cool Grub themes can be used to make it nicer like [here](https://www.gnome-look.org/p/1440862). 

* If you want to have the ability to load your previous BTRFS snapshots from the GRUB menu then `grub-btrfs` is a thing. 

    ```
    sudo pacman -S grub-btrfs
    grub-mkconfig -o /boot/grub/grub.cfg
    ```
To enable automatic updating of grub for latest snapshots, you then need to enable `grub-btrfs.path` as documented [here](https://github.com/Antynea/grub-btrfs). 

    ```
    systemctl enable grub-btrfs.path
    ```

## Next Steps and Follow Up

* It's good to start using a Wayland compositor since that is very much the future of Linux computing (many desktop enviroments have a Wayland implementation like KDE and Gnome). I stuggle however to find something analgous to XFCE4 but I will be doing some more research on my end. 
* Other tools and configurations. This isn't a complete set-up guide but it should be good enough as a functional bare-bones implementation of a daily-driver. 




