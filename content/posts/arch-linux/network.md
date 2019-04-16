---
draft: true
---
# Networking

## Get wired ethernet working with systemd-networkd:

```
# /etc/systemd/network/20-wired.network

[Match]
Name=en*

[Network]
DHCP=ipv4
```

```
sudo systemctl enable systemd-networkd.service
sudo systemctl enable systemd-resolved
```
A reboot might be required


## Adding networks / Connecting to new networks

```
sudo wifi-menu
```

## Making the WiFi connect to known networks automatically, like a civilized laptop

```
sudo pacman -S wpa_supplicant wpa_actiond
sudo systemctl enable netctl-auto@wlp4s0
```
A reboot is required

## Disconnecting from the current WiFi network

```
# kill the agent
sudo killall wpa_supplicant
```

## Reconnect to the configured preferred network
```
sudo systemctl restart netctl-auto@wlp4s0
```
