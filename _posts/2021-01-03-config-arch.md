---
layout: post
title: Rofi Configuration for Linux
date: 2021-01-03 00:00:00 +0100
categories: [linux, arch, rofi, xfce4]
tags: [linux, arch, rofi, xfce4]
header-img: "images/os_screenshot.png"
seo:
  date_modified: 2020-03-28 16:19:08 +0000
---

After several years of distro hopping, I have finally decided to comeback to Linux Arch. I love the lightweight nature and easy customisability of the distribution that makes it great for tinkering. Now every great operating system needs a killer application launcher, and for my OS I decided to use Rofi. In this blog post, I give a quick guide on how to setup Rofi. This should work for any Linux system, but I setup it up for Linux Arch running XFCE4. Hope you enjoy it! :)

![Screenshot of my Rofi setup](https://keepfloyding.github.io/images/os_screenshot.png)


# What is Rofi?

[Rofi](https://github.com/davatorium/rofi) is a tool that can be best described as a window-switcher, application launcher and dmenu replacement. I love it for its simplicity and powerful search functionalities. I have configured it so that when you press the SUPER key, you open up the drun mode to search and run an application. You can then use SHIFT + LEFT (or RIGHT) to cycle through other modes such as :
* the window swticher which lists the applications running in each workspace
* file browser to search through files
* ssh launcher to connect to other machines on my network, 

If you want to see it in action, then I can suggest the [Arch Labs](https://archlabslinux.com/) implementation of OpenBox where it is used by default. Otherwise just follow the steps below to configure it on your own system. 


# Install and configure Rofi

Installation of Rofi on Arch Linux is quite simple:

```bash
sudo pacman -S rofi
```

To hook it up with your Super key is quite difficult though, since this key is used as a modifier key to launch other applications. The way around it is to use something called [xcape](https://github.com/alols/xcape). This essentially maps your SUPER key to a more complicated set of keys which you can then implement in the key bindings of your desktop manager. So after you install xcape, you can configure your SUPER key to be something complicated like:

```bash
xcape -e 'Super_L=Shift_L|Control_L|Super_L'
```

Make sure to autostart the command above on boot so that is enabled everytime you log on. Then just add the following command for Rofi in your desktop manager's keyboard bindings but mapped to `Shift_L|Control_L|Super_L` instead of the SUPER key:

```bash
rofi -show drun -kb-cancel 'Control_L+Shift_L+Super_L'
```

The command launches the `drun` mode of Rofi and the `kb-cancel` flag is there to close Rofi when you hit the `Super` key. Make sure to select a sequence of keys that you are sure won't be used by any other keyboard shortcuts.

You can also configure other settings like enabling the usage of different modes and changing the appearance of the application by creating and editting **~/.config/rofi/config.rasi**. Here is a sample config file (shamelessly taken from the Arch Labs distro because of their super cool design):
```
configuration {
 modi: "window,drun,ssh,file-browser";
 kb-cancel: "Escape,Alt+F1";
 color-normal: "#1c2023, #919ba0, #1c2023, #a4a4a4, #1c2023";
 color-urgent: "argb:00000000, #f43753, argb:00000000, argb:00000000, #e29a49";
 color-active: "argb:00000000, #49bbfb, argb:00000000, argb:00000000, #e29a49";
 color-window: "#1c2023, #919ba0, #1c2023";
 }
 ```

 This should give your Rofi application a really cool dark mode feel. If you need any other pointers, then checkout the excellent Arch Wiki [entry](https://wiki.archlinux.org/index.php/Rofi)  Hope this was useful!