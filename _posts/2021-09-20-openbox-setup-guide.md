---
layout: post
title: Openbox GUI Setup 
date: 2021-09-28 00:00:00 +0100
categories: [linux, arch, installation, openbox]
tags: [linux, arch, installation, openbox]
---


Having the ability to choose which GUI to use with your distro is one of the great things about the Linux Desktop. I covered in a previous blog post how to setup XFCE4 with Linux Arch but I am also a very big fan of Openbox. If you are looking for the most minimal of minimal of GUIs, then look no further than Openbox. By itself, it is only a window manager and therefore doesn't have any of the tools that a typical Desktop Environment might have. This normally means that it can take longer to set up because you have to install and configure other modules in addition to Openbox. However, it makes for a great learning experience, and there is an inherent satisfaction that you can get in building a modular desktop environment from the ground up. An additional advantage to learning Openbox is that when you decide to eventually switch over to Wayland, you can make use of some of the Openbox themed Wayland compositors that have similar config files. Anyway, without further ado, here is how you can easily setup Openbox:

## Openbox Setup

* Naturally you need to install Openbox with pacman. Make sure to install Obconf, which is a GUI settings manager that you can also use to setup your system.

    ```
    sudo pacman -S openbox obconf
    ```

* Setting up Openbox typically only involves editting a few config files in `~/config/openbox`. You can get started with default settings by copying over all necessary files from `/etc/xdg/openbox`. These will contain the following:  
    * autostart (loads the programs that you need for your openbox configuration, I will talk more about this in the next point)
    * rc.xml (keyboard shortcuts and configuration themes)
    * environment (keyboard layout settings)
    * menu.xml (menu that appears when you right click on the desktop)

## GUI Setup


* There are several programs that you need alongside Openbox to have a full desktop experience. You might find some of the programs below quite useful which can be installed with `pacman`:
    * `nitrogen`: a program to set your wallpaper background
    * `picom`: a compositor that is needed since openbox doesn't have one; it does things like enable smoother transitions 
and avoid jittery behaviour
    * `tint2`: a panel for your GUI
    * `thunar`: a file manager

* Configuring `autostart`; you will need to have the following in this file to automatically launch your programs upon login.

    ```
    xfsettingsd & 
    picom & 
    (sleep 3s && nitrogen --set-centered [DIR to image] ) & 
    tint2&
    ```

* You can configure keyboard shortcuts with another package called `obkey` (installed with `yay`). This will help you to specify your desired shortcuts with a GUI. 

* Configure the themes. You can get whatever theme you desire from the web and simply download it place it in `~/.local/share/themes`. [Here](https://github.com/addy-dclxvi/openbox-theme-collections) you can find some really cool themes. After you have done this, then you can apply your chosen theme either with `obconf` or editting the `rc.xml` file as needed. 

## Panels, Menus and Applets

* I use`tint2` for the desktop panel.It's very easy to configure and works very well out of the box. I do the following modifications:
    * Add some applets to the desktop
    * Configure system tray to only show icons
    * Let the launcher be `jgmenu` for the application menu
    * Configure themes and colours

* The application menu that I use is `jgmenu`. Run `jgmenu_run init -i` for interactive configuration.

* For the app launcher I use `Ulauncher` or `rofi`. Please see my other blog posts on how to set this up.  

* Applets for sound I use `volti`. This can be configured to use `pavucontrol`. 

* Applets for networking are a bit tricky if you don't use `NetworkManager`. I would recommend using Network Manager to configure your network connection and then use `nm-applet` for the panel applet.

Honestly, that's all there is to it. Using Openbox is a learning experience and at times you might be wondering what is the point. However, the satisfication in having a minimal, modular and fully configurable GUI is trully rewarding. Additionally, you can then begin to try out some of the Wayland compositors that are heavily influenced by Openbox such as [labwc](https://github.com/johanmalm/labwc). 
