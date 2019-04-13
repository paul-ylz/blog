---
title: "Ethernet"
date: 2018-11-05T17:49:21+08:00
draft: false
---

Ethernet does not necessarily work out of the box - in which case the lights on the ethernet port will not even blink.
To enable:
See ethernet devices

{{< highlight bash >}}
$ ip link
{{< /highlight >}}

Then enable the ethernet device

{{< highlight bash >}}
$ ip link set dev <devicename> up
{{< /highlight >}}

<devicename> is the one that looks something like "enp0s31f6"

Then get DHCP working for that interface. I am able to just run

{{< highlight bash >}}
% sudo dhcpcd
{{< /highlight >}}
