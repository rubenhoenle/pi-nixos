# Raspberry Pi NixOS


## Flash the ISO to the SD card

```bash
# run this before inserting the sd card
lsblk

# NAME                                          MAJ:MIN RM   SIZE RO TYPE  MOUNTPOINTS
# sda                                             8:0    1     0B  0 disk
# sdb                                             8:16   1     0B  0 disk

# now insert the SD card, then run it again
lsblk

# NAME                                          MAJ:MIN RM   SIZE RO TYPE  MOUNTPOINTS
# sda                                             8:0    1     0B  0 disk
# sdb                                             8:16   1  29,8G  0 disk
# ├─sdb1                                          8:17   1   256M  0 part  /run/media/ruben/bootfs
# └─sdb2                                          8:18   1  29,6G  0 part  /run/media/ruben/rootfs

# NOW WE KNOW: OUR SD CARD IS /dev/sdb

# unmount the sd card
sudo umount /dev/sdb1
sudo umount /dev/sdb2

# get the path of your iso in the nix store
ls -lisa result

# write the ISO to the SD card
sudo dd if=/nix/store/xz8hw00mpcgl1y7fd4g58x1ilnpwbjkz-nixos-sd-image-24.11.20240608.cd18e2a-aarch64-linux.img/sd-image/nixos-sd-image-24.11.20240608.cd18e2a-aarch64-linux.img of=/dev/sdb bs=1M status=progress

```

