---
draft: true
---
# Setting up Not A Desktop Environment: Xorg with suckless (dwm, st, slstatus).

3 July 2018

## dwm (Dynamic Window Manager) st (simple terminal), slstatus (suckless status monitor)

It makes sense to do these 3 together as Xorg doesn't really work without a window manager, and a window manager like dwm makes no sense without a terminal.

While we are installing dw
Install Xorg with a lot of related packages
```
# pacman -S xorg
```

Since dwm won't launch x by itself we need to be able to start x ourselves
```
# pacman -S xorg-xinit
```

Install the suckless suite:
```
# clone http://git.suckless.org/dwm
# make clean install
```

```
# clone http://git.suckless.org/slstatus
# make clean install
```

```
# clone http://git.suckless.org/st
# make clean install
```

flux for linux, makes the screen easier on the eyes by reducing gamma.
```
# pacman -S redshift
```

key bindings in x, totally independent of key bindings in vconsole.
allows loading of keys through `~/.xbindkeys`

```
# pacman -S xbindkeys
```

And we are ready to fly!
`startx`

Enjoy the beautiful simplicity of dwm. Shift-Alt-P for a tty.
Shift-Alt-Q to quit dwm.

The first thing I want to install is `chromium`.

```
# pacman -S chromium
```
I can launch chromium with just `chromium` at the command line, but I want something launcher like since I'm used to Alfred.

```
# pacman -S dmenu
```
Now you can launch stuff in your $PATH with just Alt-P, depending on how you have defined MODKEY in dwm's config.h.

My Thinkpad runs at 2560x1440, which makes default fonts look tiny in most X applications... No wonder people prefer old Thinkpads with crappy displays at low resolution!  With Chromium there is an (easier) persistent solution to this:

```
# $HOME/.config/chromium-flags.conf

--force-device-scale-factor=1.4
```

Let's check out Youtube on Chromium! The first thing I notice is that there is no sound.

https://wiki.archlinux.org/index.php/sound_system

The basic ALSA drivers already seemed to be installed (inspect `lspci | grep -i audio`), so I think I just need a Sound Server. I went with the pulseaudio since that seemed most popular.

```
# pacman -S pulseaudio
```
Think we need a mixer as well to control the volume.
```
# pacman -S pamixer
```
Restart X and there is sound. Amazing!

Wallpaper in dwm :
```
# pacman -S feh

~/.xinitrc
feh --bg-scale bg.jpg
```
