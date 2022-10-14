# Arch Linux virtualization KVM
- [QEMU](#qemu)
- [Virt-Manager](#virt-manager)
## QEMU
1. https://wiki.archlinux.org/title/QEMU
```
sudo pacman -S --needed qemu-full
```

## Virt-Manager
```
sudo pacman -S --needed virt-manager qemu-desktop libvirt edk2-ovmf dnsmasq iptables-nft libguestfs
```
```
sudo systemctl enable --now libvirtd.service
```
## Configuration Virt-Manager
#### Edit the following configurations:
```
sudo nano /etc/libvirt/libvirtd.conf
```
```
...
unix_sock_group = 'libvirt'
...
unix_sock_rw_perms = '0770'
...
```
```
sudo usermod -a -G libvirt $(whoami)
```
```
newgrp libvirt
```
```
sudo systemctl restart libvirtd.service
```

#### Edit the following configurations:
Add your user to `/etc/libvirt/qemu.conf`. Otherwise, **QEMU** will give a permission denied error when trying to access local drives.

Search for `user = "libvirt-qemu"` or `group = "libvirt-qemu"`, uncomment both entries and change `libvirt-qemu` to your user name or ID. Once edited it should look something like below. 
```
sudo nano /etc/libvirt/qemu.conf
```
```
# Some examples of valid values are:
#
#       user = "qemu"   # A user named "qemu"
#       user = "+0"     # Super user (uid=0)
#       user = "100"    # A user named "100" or a user with uid=100
#
user = "username"

# The group for QEMU processes run by the system instance. It can be
# specified in a similar way to user.
group = "username"
```
