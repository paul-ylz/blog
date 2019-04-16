---
draft: true
---
# NVIDIA on T460s

Use the main nvidia driver
It is probably better to install this after setting up xorg, even if nouveau is complaining on every login.

https://wiki.archlinux.org/index.php/NVIDIA

pacman -S nvidia

Must reboot!

automatically load nvidia X settings
nvidia-xconfig

