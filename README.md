# Raspberry Pi NixOS Quickstart

This is a bare minimum config to build a NixOS Raspberry Pi SD card image which allows you to connect via SSH to your Raspberry Pi.
Then you can apply (or create) your "real" NixOS config via SSH.

Tested with a Raspberry Pi 4 with 2GB RAM.


> [!IMPORTANT]
> This bare config is adjusted to my own use-cases. The default user is named `ruben` and it includes only my SSH public key. You will have to adjust the `configuration.nix` file to fit your needs!

## Prerequisites
To be able to build the SD card image on your local x86 machine, you will have to add the following lines to your nix system config.

```nix
boot.binfmt.emulatedSystems = ["aarch64-linux"];
nix.settings.extra-platforms = config.boot.binfmt.emulatedSystems;
```

## Building the SD card image

> [!NOTE]
> This might take a while and eat up some disk space on your machine.

```bash
nix build .#nixosConfigurations.raspberry-pi_4.config.system.build.sdImage
```

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
# BE CAREFUL: The iso image is hidden in a subdirectory inside the nix store
ls -lisa result

# write the ISO to the SD card
sudo dd if=/nix/store/xz8hw00mpcgl1y7fd4g58x1ilnpwbjkz-nixos-sd-image-24.11.20240608.cd18e2a-aarch64-linux.img/sd-image/nixos-sd-image-24.11.20240608.cd18e2a-aarch64-linux.img of=/dev/sdb bs=1M status=progress
```

