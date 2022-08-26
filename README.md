# Z240 SFF Proxmox

## Enable PVE-no-Subscription

Edit /etc/apt/sources.list file and add the line:
``` bash
nano /etc/apt/sources.list
```
``` bash
deb http://download.proxmox.com/debian/pve bullseye pve-no-subscription
```

Update resitories
``` bash
apt-get update
```
Upgrade packages
``` bash
apt-get dist-upgrade -y
```

## Realtek 8156 Driver

[Link to drivers and how to](r8152-2.16.3/README.md)

## Usefull packages to install

``` bash
apt-get install lm-sensors fancontrol htop net-tools s-tui
```
