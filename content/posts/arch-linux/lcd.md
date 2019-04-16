---
draft: true
---
# Controlling the LCD (brightness) on a Thinkpad in Arch

I did this with xorg-xbacklight. If you installed Xorg with package `xorg`, you should have it already, if not
```
# pacman -S xorg-xbacklight
```
This makes /usr/bin/xbacklight available, which can control the brightness setting at /sys/class/backlight/intel_backlight/brightness.

The xbacklight binary can get the current brightness, brighten or dim the LCD display, so I map the brightness keys to it using xbindkeys.

On my T460S with dGPU (Nvidia upgrade), the setting did not work for me out of the box, I had to install the intel driver first
```
# pacman -S xf86-video-intel
```

and also this little tidbit from the backlight page:
https://wiki.archlinux.org/index.php/backlight#xbacklight
```
/etc/X11/xorg.conf
  Section "Device"
  Identifier  "Card0"
  Driver      "intel"
  Option      "Backlight"  "intel_backlight"
EndSection
```
