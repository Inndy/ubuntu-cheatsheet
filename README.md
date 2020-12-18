# Ubuntu Cheatsheet

## Configure Timezone

```sh
sudo dpkg-reconfigure tzdata
```

## Configure Default Editor & Alias

```sh
sudo update-alternative --config editor
sudo update-alternative --config vi
sudo update-alternative --config vim
```

## Configure Interface IP

```sh
sudo -e /etc/netplan/50-cloud-init.yaml && sudo netplan apply
```

### netplan template

```yaml
network:
    version: 2
    ethernets:
        enp6s18:
            addresses:
                - 1.2.3.4/24
            gateway4: 1.2.3.254
            nameservers:
                addresses:
                    - 127.0.0.1
                    - 1.1.1.1
                    - 8.8.8.8
        enp6s19:
            addresses:
                - 172.16.1.1/24
```

### Expand VM Guest Disk (LVM)

```sh
# 1. Fix partition table meta-data
# /dev/vda is the disk we expanded in VM host
sudo parted /dev/vda
# 2. Resize partition
# Re-create last partition, extend partition size
sudo fdisk vda
# 3. Resize PV
# /dev/vda3 holds the PV
sudo pvresize /dev/vda3
# 4. Resize LV and Parition
# `-r` means run resize2fs after resizing LV
sudo lvextend -l +100%FREE /dev/vg0/lv-0 -r
```
