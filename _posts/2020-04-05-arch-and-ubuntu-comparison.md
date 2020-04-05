---
layout: post
title: Linux Arch and Ubuntu - What separates these 2 giants?
date: 2020-04-05 00:00:00 +0100
categories: [Programming, Linux, Arch]
tags: [Programming, Linux, Arch, Ubuntu]
---

I have been a regular Linux user for some years now. My journey started with dual booting my laptop with Ubuntu since I still needed to use Windows for my studies. I ended up using Ubuntu full time before graduating to Linux Mint with Cinnamon environment and then like all curious Linux users, I started dabbling with Linux Arch. I tried to keep up with it, but the lack of stability made me switch back to Ubuntu. Now several years later, I am switching back to ARch bit via the Archlabs way. However, a lot of my distro hopping focussed around what looked good and not about the fundamentals of the system. So here in an effort to undersatnd what makes Arch so diffeent from debian based systems (Ubunut in particular), I have wirtten this blog post. I wont be talking about Fedora and I have no real exposure to it. 

# A Brief Introduction 

Ubuntu is a debian based OS supported by Canonical. They market themselves to newbies and the general public making Linux accessible to all. The fact that they have a cool name helps a lot ("Ubuntu" is a  "a quality that includes the essential human virtues; compassion and humanity." in [Nguni](https://en.wikipedia.org/wiki/Nguni_languages) languages). 

Arch Linux is very different. Was originally built by some guy as a bare bones distro for those wanting to dive head first into Linux. Their key motto is KISS (Keep It Simple Stupid?).

So as a summary; you could say that its a battle between stability and bleeding edge. But I think that there is more to this. 

# Summary of Key Differences

So what are they key differences between using and arch system and Ubuntu. In my mind, they can be summarised as follows:

* Pacman vs. apt-get
* Less bloat
* Rolling release vs. cycles?
* You can say that you use Arch 
* Customisability
* The 'Arch' way

I will go through each of these points in the next sections. 

# A War of Package Managers

Each distro has its own unique package manager (`apt` for Ubuntu and the excellently named `pacman` for Linux Arch). Now what exactly makes them different and more importantly which one is better. I guess there are several different points to this:

* Speed
* Ease of Use
* Extensibility 
* Hackability

The first 2 points I think are moot. They are both easy to use with no real difference in commands for the installation and removal of packages and their speeds, albeit different, are still relatively quick in the context of what either of them requires. The extensibility is also very similar. Although there are more official packages for Ubuntu due to the larger audience and team that drives its development; if we take into account the AUR (Arch User Repository) it is basically comparable. Now I think that really there is a big difference in terms of hackability. Pacman is notorious for being easy to set-up to install any custom user defined package. If there is a .deb file, then some small modifications allow Arch to use it as well. In fact, this ability has allowed for the creation of the AUR and allowed Linux Arch to have the extensibility that it currently has. Therefore, pacman essentially allows the Linux Arch distro to become what it truly wants to be (a usable distro for Linux users, built by Linux users). There is something so eye-tearingly democratic about that. 

# Philosophy 

Arch is about a bare-bones install as much as realistically possible. You can always go the whole hog and do a linux from scratch installation; but that would rather be more of a learning experience rather than anything else. So when you get your arch install, there is literally no bloat in your system. This bare bones approach also makes it highly configurable; you choose what you want. 

Ubuntu is different. It is all about ease-of-use and stability. It comes with a preconfigured desktop environment. 

# Rolling release vs. bleeding edge

# Systemd

# Installation and learning

All about the Arch way

# What are the alternatives?

* Manjaro
* Arch labs

# Concluding remarks

I used Ubuntu and loved it. GNOME desktop is elegant and Ubuntu is super stable. 
Gradually going to Arch via Archlabs because it has the following:
* Minimal desktop
* Learning approach
* No bloat

For the newbie, I would definitely recommend Ubuntu, for the intermediate Archlabs/Manjaro and for the die-hard fan, Linux Arch the hard way. 
