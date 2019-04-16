---
draft: true
---
# Neovim on Arch

I moved from Vim to Neovim earlier this year (2018). I'm still not sure why... it feels a little slower than Vim, and I don't like how Python is a dependency... but it still has much lower latency than VSCode and somebody on Reddit talked about why giving Neovim a chance was important for progress, innovation, async and all that and that kind of sold me on giving it a shot.

Neovim is available as an official package on Arch.
https://wiki.archlinux.org/index.php/Neovim

```
   # pacman -S neovim
```

Neovim is not going to work right without python2 and python3. If you run `:checkhealth` there will be some errors about this if both pythons aren't installed.
Further, both pythons will need to have the neovim module.

Python 3:
```
# pacman -S python
# pacman -S python-pip
# pip install neovim
```

Python 2:
```
# pacman -S python2
# pacman -S python2-pip
# pip install neovim
```
If the pythons and their neovim modules have been installed, neovim will be happy when running `:checkhealth`.

Install Vim Plug and it's off to the good life
https://github.com/junegunn/vim-plug

```
curl -fLo ~/.local/share/nvim/site/autoload/plug.vim --create-dirs \
    https://raw.githubusercontent.com/junegunn/vim-plug/master/plug.vim
```

