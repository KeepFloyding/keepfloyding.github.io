Part 2 of Arch setup (Graphical enviro and other things)


17. Install a few other things

    ```
    sudo pacman -S git openssh redshift code nano vim firefox xcape rofi xfce4 xclip xorg linux-firmware file-roller gedit conky lightdm
    ```


20. Choose Xfce Session from the menu in a display manager of choice, or add exec startxfce4 to Xinitrc.

21. Configure Audio. Using pulse-audio and pavu control as the front-end. XFCE4 also has a plugin for pulse audio that can be installed.

```
sudo pacman -S pulseaudio pavu-control
```

22. Configure bluetooth with bluez and bluez-utils. Bluetooth can then be configured with `bluetoothctl`.
