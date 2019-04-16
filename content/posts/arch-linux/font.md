---
title: "Fonts in Arch"
date: 2018-11-05T17:49:21+08:00
draft: false
---
It's pretty easy to set font in the Virtual Console.
{{< highlight bash >}}
pacman -S terminus-font
setfont ter-v32n
{{< /highlight >}}

Voila, eyes saved.

I found it a bit trickier setting the font within X / dwm / st.

On the T460S I used X with dwm as the window manager, and st (simple terminal) to get the all important tty.
Initially, the fonts inside my ttys were screwy - as in microscopic and also the kerning was totally wack.

I managed to recompile st using a different font size but for some reason was unable to change the font to terminus, or to anything else.
I kept getting some other sans-serif font with weird kerning.

Thanks to the good folks `Khorne`, `MrElendig` on IRC (chat.freenode.net #archlinux-newbie) I learnt how to properly identify the font in st's config.h.

{{< highlight bash >}}
fc-list | grep -i terminus
{{< /highlight >}}

This would give me a number of lines that look like:

{{< highlight bash >}}
/usr/share/fonts/misc/ter-x32n.pcf.gz: xos4 Terminus:style=Regular
{{< /highlight >}}

The key here is "xos4 Terminus", which is the name by which to identify the font with in config.h.
```bash
static char *font = "xos4 Terminus:pixelsize=24:antialias=true:autohint=true";
```

Compile and reinstall
```bash
# generate new checksum
makepkg -g » PKGBUILD
# force, install
makepkg -fi
```

## Other commands discovered
List installed fonts
```bash
fc-list
```

fc-match matches a given font to the argument.
Basically if fc-match does not recognize your argument, your font won't be recognized.
```bash
fc-match terminus
=> 1048013t.pfa: "Luxi Sans" "Regular"
```

```bash
fc-match "xos4 Terminus"
=> ter-x12n.pcf.gz: "xos4 Terminus" "Regular"
```

Cool!

