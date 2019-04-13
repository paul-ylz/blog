---
draft: true
---
# A password manager for Arch

I used gopass for OSX, and I love it. I decided to go with the original pass, written in bash on Arch. And there is an official pacman entry for it!

https://wiki.archlinux.org/index.php/Pass

```
# pacman -S pass
```

If you don't already have bash completion installed:
```
# pacman -S bash-completion
```

and set up
```
~/.bash_completion
source /usr/share/bash-completion/completions/pass
```

`dmenu` also works well with providing a menu interface to pass using /usr/bin/passmenu, which is pretty cool too.
