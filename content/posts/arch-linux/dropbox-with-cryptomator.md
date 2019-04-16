---
title: "Installing Dropbox w Cryptomator"
date: 2018-11-05T17:49:21+08:00
draft: false
---
https://wiki.archlinux.org/index.php/Dropbox

{{<highlight bash>}}
pacman -S dropbox
{{</highlight>}}

I just run Dropbox when I'm in Xorg, so using ~/.xinitrc:

{{<highlight bash>}}
/usr/bin/dropbox &
{{</highlight>}}

The next time you start X, Dropbox will pop open a browser to sign in with. After you sign in, ~/Dropbox will just automatically sync.

Cryptomator lets you store encrypted vaults on Dropbox. Unlocked vaults are made available via webdav, so some type of webdav client is needed. I tried nautilus and it worked, so I am using that. I've used Cryptomator for a while now and it worked perfectly for me on OSX and made things accessible on my Android phone via the Cryptomator app.

{{<highlight bash>}}
cd /usr/local/src
git clone https://aur.archlinux.org/cryptomator.git
makepkg -sri
{{</highlight>}}

Although the Cryptomator installs open-jdk (open source Java) as a dependency, it would not start on my machine with a "Graphics Device initialization failed for: es2, sw. Error initializing QuantumRenderer: no suitable pipeline found".

I got around this issue by installing Oracle Java through AUR.

{{<highlight bash>}}
git clone https://aur.archlinux.org/jre.git
cd jre
makepkg -sri
sudo archlinux-java set java-10-jre/jre
{{</highlight>}}

And Cryptomator runs!

After unlocking Cryptomator vaults, they become available via WebDAV.
In order to edit files in the vault, they have to be mounted. Cryptomator gives you an easy way to copy the WebDAV URL, and I find it convenient to use aliases to to mount the vaults. In order to mount a DAV, you need to be able to use `davfs` a dav filesystem, which means we need `davfs2`.

{{<highlight bash>}}
pacman -S davfs2
{{</highlight>}}

I have aliases that look like
{{<highlight bash>}}
alias mntsomething='sudo mount -t davfs http://localhost:42427/someuuid/myvaultname /mnt/myvaultname -o username=${USER},rw,uid=${USER},file_mode=700,dir_mode=700'
{{</highlight>}}

As you modify files inside the mounted vault, Cryptomator updates the vault while you do so, and the encrypted files get synced to Dropbox on the fly.

