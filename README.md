ArchlinuxARM for the OLinuXino
===

This repository includes packages needed to support ArchLinuxARM on the excellent [OLinuXino](http://www.olimex.com/dev/oli-main.html) boards made by [Olimex](http://www.olimex.com/) along with a script to generate a modified rootfs. See below for prebuilt images. This is a work in progress!

There's also a repository for the packages, included in `pacman.conf` within the 
prebuilt image. The repository url is [http://1024.cjb.net/archlinux/olinuxino/](http://1024.cjb.net/archlinux/olinuxino/).

Installation
---

The easiest way to put Archlinux ARM on your Olinuxino board is to use the prebuilt boot image and root file system.

* Download the files.
  * [olinuxino-boot.img](http://1024.cjb.net/archlinux/olinuxino-boot.img) md5sum: a10b71fe436bf9706737145d9765a639
  * [olinuxino-alarm-2012.07-rootfs.tar.gz](http://1024.cjb.net/archlinux/olinuxino-alarm-2012.07-rootfs.tar.gz) md5sum: 8266f0c3bf31691fc4fe0c3c056df030

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
* PKGBUILDs for packages needed to support the Olinuxino boards, only Maxi is
  tested, but other iMX233 variants should boot smoothly too. The PKGBUILDs and
  accompanying files can be found under `olinuxino/`.
* A script to generate a modified root file system, under `scripts`. Minor 
  changes of system configuration packages are required to boot alarm properly
  on the board:
  * The console device is ttyAMA0 instead of ttyS0;
  * A custom rc script sets the MAC address of the eth interface;
  * The default locale is ISO-8859-1, 64MB of ram are not sufficient to 
    generate UTF-8 locales. The workaround is to enable some swap for locale-gen.

Generating your own
---

Everything here works on an Archlinux ARM host. It should be possible to use the Olinuxino board itself, after installing the prebuilt rootfs, but this will probably be painfully slow. A qemu host with alarm or another ARM board would work much better.

The ArchlinuxARM website has guides on setting up distcc, and distcc with cross-compiling workers. The latter allows using your powerful workstations to do the heavy number crunching. According to alarm's devs, this approach is fine for testing and developing new stuff, but they always build their official packages natively.
  
For info on what PKGBUILD is and how to build your own packages, see the [docs](http://archlinuxarm.org/developers/) on the [Archlinux ARM](http://archlinuxarm.org/) website, as well as the excellent documentation of [the upstream distro](https://wiki.archlinux.org/)

License
---

PKGBUILDs featured here use code from [Freescale](http://www.freescale.com/) and code and patches from [OSSystems](https://github.com/OSSystems/) and their [Yocto layer](https://github.com/OSSystems/meta-fsl-arm-extra) for Olinuxino and code from [ArchlinuxARM's repository](https://github.com/archlinuxarm/PKGBUILDs). Those have their respective licenses. Everything else (i.e. the PKGBUILDs here) is public domain.
