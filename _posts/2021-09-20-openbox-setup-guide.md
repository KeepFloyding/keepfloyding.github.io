---
layout: post
title: Openbox GUI Setup 
date: 2021-09-28 00:00:00 +0100
categories: [linux, arch, installation, openbox]
tags: [linux, arch, installation, openbox]
---

Having the ability to make a choice regarding which GUI to setup with your Linux distro is one of the fun things of messing with Linux. I have covered in previous blog posts how to setup XFCE4 with Linux Arch but I am also a very big fan of Openbox. If you are looking for the most minimal of minimal of GUIs, then look no further than Openbox. In of itself, it is simply a window manager and doesn't have the tools of a typical Desktop Environment. This means that it will take longer to set up but you can choose whatever modules that you want to run with it. This makes for a great end experience because you have put your environment together like lego blocks. An additional advantage to learning about Openbox is that when switching over to Wayland, you can make use of some of the Openbox themed Wayland compositors that have similar config files. Here are some of the steps that you can take to setup Openbox. 

## Setup

* Naturally you need to install Openbox with pacman. Make sure to install Obconf, which is a GUI settings manager that you can also use to setup your system.

    ```
    sudo pacman -S openbox obconf
    ```

* Setting up Openbox typically only involves editting a few config files in `~/config/openbox`. You can get started with default settings by copying over all necessary files from `/etc/xdg/openbox`. These will contain the following:  
    * autostart (loads the programs that you need for your openbox configuration, I will talk more about this in the next point)
    * rc.xml (keyboard shortcuts and configuration themes)
    * environment (keyboard layout settings)
    * menu.xml (menu that appears when you right click on the desktop)

* There are several programs that you need for your openbox theme. These can be installed with pacman as usual. You might find some of the programs below quite useful:
    * `nitrogen`; a program to set your wallpaper background
    * `picom`; a compositor that is needed since openbox doesn't have one; it does things like enable smoother transitions 
and avoid jittery behaviour
    * `tint2`; a panel for your GUI


* Configuring autostart; you will need to have the following to automatically launch your programs.

    ```
    xfsettingsd & 
    picom & 
    (sleep 3s && nitrogen --set-centered [DIR to image] ) & 
    tint2&
    ```

* You can configure keyboard shortcuts with another package called `obkey` (installed with yay). This will help you to specify your desired shortcts with a GUI. 

* Configure the themes. You can get whatever theme you desire from online and simply download it place it in `~/.local/share/themes`. [Here](https://github.com/johanmalm/labwc) you can find a nice list of websites where you can find some really cool themes. After you have done this, then you can apply the theme either with `obconf` or editting the `rc.xml` file as needed. 

* Configuring `tint2`. Very easy to configure and works very well out of the box. I do the following mods:
    * Add some applets to the desktop
    * Configure system tray to only show icons
    * Let the launcher be `jgmenu` for the application menu
    * Configure themes and colours

* The application menu that I use is `jgmenu`. Run `jgmenu_run init -i` for interactive configuration.

* For the app launcher I use `Ulauncher` or `rofi`. Please see my other blog posts for how to set this up.  

* Applets for sound I use `volti`. This can be configured to use `pavucontrol`. 

* Applets for internet are a bit tricky if you don't use `NetworkManager`. I would recommend using Network Manager to configure your network connection and then use `nm-applet` for the panel applet.

Honestly, that's all there is to it. Using Openbox is a learning experience and at times you might be wondering what is the point. However, the satisfication in having a minimal, modular and fully configurable GUI is trully rewarding. Additionally, you can then begin to try out some of the Wayland compositors that are heavily influenced by Openbox such as [labwc](https://github.com/johanmalm/labwc). 
