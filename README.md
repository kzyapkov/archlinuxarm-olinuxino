ArchlinuxARM for the OLinuXino
===

This repository includes packages needed to support ArchLinuxARM on the excellent [OLinuXino](http://www.olimex.com/dev/oli-main.html) boards made by [Olimex](http://www.olimex.com/) along with a script to generate a modified rootfs.

Installation
---

The easiest way to put Archlinux ARM on your Olinuxino board is to use the prebuilt boot image and root file system.

* Download the files.
  * [olinuxino-boot.img](http://1024.cjb.net/archlinux/olinuxino-boot.img)
  * [olinuxino-alarm-2012.07-rootfs.tar.gz](http://1024.cjb.net/archlinux/olinuxino-alarm-2012.07-rootfs.tar.gz)

* Partition your sdcard.
  You need two partitions with the following layout:
  ```
            Device Boot      Start         End      Blocks   Id  System
    /dev/mmcblk0p1   *          16       31231       15608   53  OnTrack DM6 Aux3
    /dev/mmcblk0p2           31232     3842047     1905408   83  Linux
    ```
  Step-by-step instructions on how to achieve this partition layout can be 
  found [here](https://github.com/radolin/meta-olinuxino).

* Put the images on the sdcard.
  These instructions assume that the SD card is visible on your machine
  as `/dev/mmcblk0`. This may be `/dev/sdc` or somehting else completely, so
  please check before you run these commands. To put the boot image on the 
  first partition, do (maybe as root):
  ```
  $ dd if=olinuxino-boot.img of=/dev/mmcblk0p1
  ```
  Next, extract the root file system onto the second partition (again, this
  may require root):
  ```
  $ mkfs.ext3 /dev/mmcblk0p2
  $ mount /dev/mmcblk0p2 /mnt/mmc # or somewhere else
  $ tar -C /mnt/mmc -xzf olinuxino-alarm-2012.07-rootfs.tar.gz
  $ umount /dev/mmcblk0p2
  ```
* Put the sdcard into the olinuxino and power on! The default `root` password 
  is `root`.

This Repository
---

Featured here:
* PKGBUILDs for packages needed to support the Olinuxino boards, only Maxi at 
  this point. The PKGBUILDs and accompanying files can be found under 
  `olinuxino`. For info on what PKGBUILD is and how to build your own packages, 
  see the [docs](http://archlinuxarm.org/developers/) on the 
  [Archlinux ARM](http://archlinuxarm.org/) website, as well as the excellent 
  documentation of [the upstream distro](https://wiki.archlinux.org/)
* A script to generate a modified root file system, under `scripts`. Minor 
  changes of system configuration packages are required to boot alarm properly
  on the board:
  * The console device is ttyAMA0 instead of ttyS0;
  * A custom rc script sets the MAC address of the eth interface;
  * The default locale is ISO-8859-1, 64MB of ram are not sufficient to 
    generate UTF-8 locales. The workaround is to enable some swap for locale-gen.



License
---

PKGBUILDs featured here use code from Freescale and code and patches from OSSystems and their Yocto layer for Olinuxino and code from ArchlinuxARM's repository. Those have their respective licenses. Everything else (i.e. the PKGBUILDs) is public domain.
