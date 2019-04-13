---
draft: true
---
# Remapping of keys has to be done in 2 places:

## /etc/vconsole.conf for virtual console

I really need Caps Lock to be a Control key. I feel incapacitated without it.

```
# cd /usr/local/share
# mkdir -p kbd/keymaps
# cp /usr/share/kbd/keymaps/us.tar.gz /usr/local/share/kbd/keymaps/personal.tar.gz
# cd /usr/local/share/kbd/keymaps
# gunzip personal.tar.gz
# vim personal.map

change line that says `keycode 58 = Caps_Lock` and to `keycode 58 = Control`

# test this configuration with
loadkeys /usr/local/share/kbd/keymaps/personal.map
```

If all is ok, make it load automatically with:

```
# /etc/vconsole.conf
KEYMAP=/usr/local/share/kbd/keymaps/personal.map
```

Keymap is active in tty sessions, but not in X.

## To do the same in X, one can use xmodmap, like so:

```
# ~/.xinitrc
# from https://www.x.org/archive/X11R6.8.1/doc/xmodmap.1.html
xmodmap -e "remove Lock = Caps_Lock"
xmodmap -e "remove Control = Control_L"
xmodmap -e "keysym Control_L = Caps_Lock"
xmodmap -e "keysym Caps_Lock = Control_L"
xmodmap -e "add Lock = Caps_Lock"
xmodmap -e "add Control = Control_L"

# start window manager
```

Alternatively use `xbindkeys`.
```
# pacman -S xbindkeysxbindkeys -d > ~/.xbindkeysrc
```
In addition to rebinding Caps Lock as a Control key, I also set the Mute / Volume buttons, brightness settings, clipmenu and screenshot bindings with xbindkeys.
https://github.com/paul-ylz/dotfiles/blob/master/.xbindkeysrc

