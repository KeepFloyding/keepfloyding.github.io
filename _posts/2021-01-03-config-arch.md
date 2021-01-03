---
layout: post
title: Configuration of Linux Arch System
date: 2021-01-03 00:00:00 +0100
categories: [Linux, Linux Arch, XFCE, OS]
tags: [Linux, Linux Arch, XFCE, OS]
header-img: "images/archbtw.png"
seo:
  date_modified: 2020-03-28 16:19:08 +0000
---

To celebrate the New Year, I decided to return my personal desktop computer to Arch and in doing so I was looking at what were the best implmentations of Arch out there. I primarily looked at 3 different implementations: Endeavour OS, Arco Linux and Arch Labs. What I came away with from all 3 implementations is an appreciation how awesome they made these custom distros look. I decided to build my own Linux Arch system but borrow heavily from the customisation of all distros. Here is what the end result looks like:

![](https://keepfloyding.github.io/images/os_screenshot.png)

Here are just a few simple steps that you need to follow:

# Install Linux Arch with XFCE4

Follow the installation guide and some external resources in getting a minimal Linux Arch installation on your computer. For the graphical environment, I chose to go with Xfce4 although I am a huge fan of the openbox window manager (simply because I couldn't be bothered to get everything up and running). For the dispaly manager, I decided to go with LightDM and the GTK greeter.  

# Install and configure rofi

Now one of the things that I thought made the Openbox implementation of ArchLabs super compelling was the use of rofi as an application launcher. Here is the GitHub link explaining what it is used for. I primarily use it as an application launcher, but if you press shift arrow key, you can access other great features such as a file manager, a window viewer, an SSH client amongst many other things. To hook it up with your Super key is quite difficult though, since often times this key is used as a modifier key to launch other applications. The way around it, is to use something called xcape. This essentially maps your window key to a more complicated set of keys which you can then implement in the key bindings. So the commands to configure it are:

```bash
xcape -e 'Super_L=Shift_L|Control_L|Super_L'
```

In xfce4, you need to set this to autostart so that you don't need to manually enter this command everytime you reboot. Then just add the following command for rofi in your keyboard bindings but mapped to `Shift_L|Control_L|Super_L`:

```bash
rofi -show drun -kb-cancel 'Control_L+Shift_L+Super_L'
```

The command launches the `drun` mode of rofi and the `kb-cancel` flag is there to close rofi when you hit the `Super` key. Make sure to select a sequence of keys that you are sure won't be used by any other applications.

# Modify GRUB menu to search for other distros

I also had to do some mods to the grub start-up since I had some other operating systems living on disk. To achieve this, all that is required is a module known as `osprober` and before you run the mkgrub command, simply mount the root partition of your other operating systems like so:

```bash
mount /dev/sdx1
grub-mkconfig -o /boot/grub/grub.cfg
```

Then the output of the above commands should tell you if some other OSs were found. 