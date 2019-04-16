---
draft: true
---
# Keeping time in sync with Arch

```
# systemctl enable systemd-timesyncd
# systemctl start systemd-timesyncd
// time should be synced now
# timedatectl list-timezones
# timedatectl set-timezone Asia/Singapore
// system clock should show the localtime
```
