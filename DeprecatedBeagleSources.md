# Beagle Source Code for revisions below REV B #

**The source uploaded are:**

  * X-loader     : http://beagleboard.googlecode.com/files/x-load-beagle-rev2.tar.gz.gz
  * U-boot (1.1.4) : http://beagleboard.googlecode.com/files/u-boot-beagle-rev2-trial2.tar.gz.gz
  * Linux Kernel (2.6.22)   :  http://www.beagleboard.org/uploads/2.6_kernel-beagle-rev2.tar.gz

**The Pre-Built Images are available for:**

  * x-load.bin                      : http://beagleboard.googlecode.com/files/x-load.bin

> "Stable slow" MLO + u-boot.bin(381 MHz & L2 cache for kernel disabled):
  * MLO                             : http://beagleboard.googlecode.com/files/MLO (md5sum: 6a9f907d630de81f0b8ee8398cf94cf6)
  * u-boot.bin                      : http://beagleboard.googlecode.com/files/u-boot.bin (md5sum: 249bb0e452b60dce6560fbb54c4de844)

> "Experimental performance" MLO + u-boot.bin (500 MHz & L2 cache for kernel enabled):
  * MLO                             : http://www.beagleboard.org/uploads/20080428/MLO (md5sum: fd4bd040dd000158952b67b743a5eb9c)
  * u-boot.bin                      : http://beagleboard.googlecode.com/files/u-boot.bin_500 (rename to u-boot.bin) (md5sum: 93fcfe00d952ebcd2c8cea8ce3232946)

  * Kernel (uImage) 2.6.22          : http://beagleboard.googlecode.com/files/uImage
  * BusyBox (ramdisk) File System   : http://beagleboard.googlecode.com/files/rd-ext2.bin
  * BusyBox FS with ALSA libraries  : http://beagleboard.googlecode.com/files/ALSA-FS.tar.gz

**Tools used      :**

_For Compilation_

http://www.codesourcery.com/gnu_toolchains/arm/download.html

_For Signing_

http://beagleboard.googlecode.com/files/signGP

_HP USB Disk Storage Format Tool 2.0.6_

http://selfdestruct.net/misc/usbboot/SP27213.exe

**Compilation Steps**

_Compiling x-loader_

> make CROSS\_COMPILE=arm-none-linux-gnueabi- distclean

> make CROSS\_COMPILE=arm-none-linux-gnueabi- omap3530beagle\_config

> make CROSS\_COMPILE=arm-none-linux-gnueabi-

_Compiling u-boot_

> make CROSS\_COMPILE=arm-none-linux-gnueabi- distclean

> make CROSS\_COMPILE=arm-none-linux-gnueabi- omap3530beagle\_config

> make CROSS\_COMPILE=arm-none-linux-gnueabi-

_Compiling Kernel_

> make CROSS\_COMPILE=arm-none-linux-gnueabi- distclean

> make CROSS\_COMPILE=arm-none-linux-gnueabi- omap3\_beagle\_defconfig

> make CROSS\_COMPILE=arm-none-linux-gnueabi- uImage


**Convert x-load.bin to MLO (required for MMC Boot)**

  1. Use the "SignGP" tool to sign the x-loader image. (“x-load.bin.ift” file is generated in the same folder.)
> > ./signGP x-load.bin
  1. Rename x-load.bin.ift to MLO
  1. Copy MLO to MMC/SD card using a card reader/writer.