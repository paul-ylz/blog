---
title: "Copying and Pasting in Arch linux with Suckless Simple Terminal"
date: 2019-04-13T17:32:26+08:00
draft: false
---

Patch Simple Terminal so that there is just a single clipboard.
https://st.suckless.org/patches/clipboard/

If like me, you had never patched a C file before...
```bash
cd src/st-0.8.1
wget https://st.suckless.org/patches/clipboard/st-clipboard-0.8.1.diff
patch < st-clipboard-0.8.1.diff
```
Amazing!


If like me, maybe you also weren't aware that copy and paste inside ST uses the keys:
Control-Shift C (copy) and Control-Shift V (paste)

Install Chris Down's clipmenu (AUR)
clipnotify (AUR) is a dependency of clipmenu. With AUR, dependencies that are also AUR / community sourced have to be installed manually.

```bash
# git clone https://aur.archlinux.org/clipnotify
# cd clipnotify
# makepkg -sri
# cd ..
# git clone https://aur.archlinux.org/clipmenu
# cd clipmenu
# makepkg -sri
```

clipmenu requires a daemon, which can be run with systemd using the script included on the github README.
Has to run on the user service manager as opposed to the system manager. Do not run the following as root!

```bash
mkdir -p ~/.config/systemd/user
cd ~/.config/systemd/user
wget https://raw.githubusercontent.com/cdown/clipmenu/develop/init/clipmenud.service
systemctl enable --user clipmenud.service
systemctl start --user clipmenud.service
```

Now to inspect the clipmenu, you need to fire the command <code>clipmenu</code>.
[I bind it to the `PrtScr` button in .xbindkeys] (https://github.com/paul-ylz/dotfiles/blob/77d35f06ad105239929ea813903859fc2c6573f1/.xbindkeysrc)

Run `/usr/bin/xbindkeys` to reload `~/.xbindkeysrc`

Clipmenu should be working at this point. Something I like about (Linux? X?) is that copy and paste works without having to press Control-C. Try this in Chromium... just highlight some text with your mouse, then use Clipmenu and you'll see the text that you highlighted.

Since Clipmenu stores multiple entries and we often copy things like passwords, deleting the clipboard is something we'll do regularly. Clipmenu comes with the command `clipdel -d <regex>` to delete clipmenu items, so I have `clipdel -d .*` bound to an alias in my bashrc.
