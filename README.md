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

Install packages required to build the kernel if you have not done so already in the previous step:
``` bash
apt-get install libelf-dev build-essential pve-headers-`uname -r`
```

Build the kernel module:
``` bash
make -j2
```

Verify the new module:
``` bash
modinfo ./r8152.ko
```

If everything looks fine, install the module:
``` bash
make install
```

Update the kernel module dependency file:
``` bash
depmod -a
```


Create a new file in modeprobe.d:
```bash
vi /etc/modprobe.d/rtl_usb.conf
```

Add this line and save it:
```
alias usb:v0bdap8156d*dc*dsc*dp*ic*isc*ip*in* r8152
```
> Vendor ID and Product ID may differ if using a different Realtek device.

Also add the udev rules file:
``` bash
cp 50-usb-realtek-net.rules /etc/udev/rules.d/
```

Update the kernel module dependency file:
``` bash
depmod -a
```

Blacklist the cdc_ether module by editing the following file:
``` bash
vi /etc/modprobe.d/blacklist.conf
```

And adding this line:
```
blacklist cdc_ether
```

Disable USB autosuspend using Grub by editing the following file:
``` bash
vi /etc/default/grub
```

Ensure that the GRUB_CMDLINE_LINUX_DEFAULT value contains usbcore.autosuspend=-1. As an example:
```
GRUB_CMDLINE_LINUX_DEFAULT="usbcore.autosuspend=-1 quiet"
```

Update grub and initramfs and if everything went well, reboot the machine:
``` bash
update-grub
update-initramfs -v -u
shutdown -r now
```

## AMT enable Serial-Over-Lan

Add following lines to /etc/default/grub:
```
GRUB_CMDLINE_LINUX="console=tty0 console=ttyS4,115200n8"

GRUB_TERMINAL=serial
GRUB_SERIAL_COMMAND="serial --speed=115200 --port=0xf0e0 --word=8 --parity=no --stop=1"
```

Update GRUB:
``` bash
update-grub
```

## Usefull packages to install

``` bash
apt-get install lm-sensors fancontrol htop net-tools s-tui
```
