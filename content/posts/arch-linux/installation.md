---
title: "Arch Linux installation"
date: 2018-11-05T17:49:21+08:00
draft: true
---
For a guide on installing on an encrypted volume see : 
https://www.youtube.com/watch?v=rT7h62OYQv8&feature=youtu.be&t=275

1. Install font to save your eyes!
```bash
pacman -Sy terminus-font
setfont ter-v32n
```


```bash
cat /proc/partitions
```

2. Check out block devices
```bash
lsblk
```

3. Using gdisk
```bash
% gdisk /dev/sda
> 2 (GPT)
> o
> y
>
```

4. cryptsetup -y -v luksFormat /dev/sda2

5. lsblk -f

6. cryptsetup open /dev/sda2 cryptroot

7. mkfs.ext4 /dev/mapper/cryptroot

8. mount /dev/mapper/cryptroot /mnt

9. mount /dev/sda2 /mnt/boot
where /dev/sda2 is EFI partition

10 pacstrap /mnt base base-devel mkinitcpio-nfs-utils cryptsetup lvm2 intel-ucode

11. arch-chroot /mnt

12. bootctl install

13. vim /etc/mkinitcpio.conf

14. vim /boot/loader/loader.conf

15. mkinitcpio -p linux

## Generate file system tables
genfstab -p -U /mnt > /mnt/etc/fstab

then

arch-chroot /mnt

## echo en_US.UTF-8 UTF-8 > /etc/locale.gen

run locale-gen
## locale-gen

kbdrate  # adjust the keyboard rate
showkey # displays the keycode of the depressed key

KAI HENDRY:
fetching UUIDs using `blkid`
## /boot/loader/entries/arch.conf
title Arch Linux
linux /vmlinuz-linux
initrd /intel-ucode.img
initrd /initramfs-linux.img
options luks.uuid=694c61ac-1927-46e7-bf76-e52d9e30f5bd luks.name=slash root=UUID=80d64475-0722-452e-93c9-e9fe8c218e92 rw

# /etc/mkinitcpio.conf
HOOKS="base udev autodetect modconf block filesystems keyboard encrypt fsck"

should be this using systemd:
HOOKS=(base systemd autodetect keyboard sd-vconsole modconf block sd-encrypt sd-lvm2 filesystems fsck)

# /boot/loader/loader.conf
timeout 3
default arch

# /etc/vconsole.conf
FONT=ter-v32n
