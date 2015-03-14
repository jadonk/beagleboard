# **Latest code moved!** #

**Code has moved to [github](http://github.com/beagleboard)**

# Beagle Board REV B and C Software Components #

For Beagle Board revision B and C software components see [BeagleBoard revision C validation page](http://code.google.com/p/beagleboard/wiki/BeagleboardRevCValidation).

Please don't use below code and binaries any more, they are outdated!

## Outdated REV B source for Beagle Board ##

| **Source Archive** | **md5sum** |
|:-------------------|:-----------|
| [u-boot 1.3.3](http://www.beagleboard.org/uploads/u-boot_beagle_revb.tar.gz) |  |
| [Linux Kernel 2.6.22.18](http://www.beagleboard.org/uploads/2.6_kernel_revb-v2.tar.gz) | 57856a49270b3e239920cf64c7bcb9bc |

## Outdated Pre-Built Images for Beagle Board REV B ##

| **Pre-Built Image** | **Description** | **md5sum** |
|:--------------------|:----------------|:-----------|
|[BusyBox (ramdisk) File System](http://beagleboard.googlecode.com/files/rd-ext2-8M.bin) | 8MB Ramdisk File System Image |  |
| [BusyBox FS with ALSA libraries](http://beagleboard.googlecode.com/files/ALSA-FS.tar.gz) |File system Image with ALSA libraries |  |

## Tools for OMAP3 Beagle Board ##

| **Tool Path**                    |                 **Description** |
|:---------------------------------|:--------------------------------|
|[ARM Linux GCC ](http://www.codesourcery.com/sgpp/lite/arm/portal/release858)          |codesourcery tool chain (Select GNU/Linux and download [version 2009q1-203](http://www.elinux.org/ARMCompilers))|
|[DSP C6x Free Compiler](https://www-a.ti.com/downloads/sds_support/targetcontent/LinuxDspTools/download.html) | DSP C64x Linux Based Compiler |
|[signGP](http://beagleboard.googlecode.com/files/signGP)           |x-loader Signing Tool [Source is here](http://beagleboard.googlecode.com/files/signGP.c)|
|[HP MMC/SD Disk Format Tool](http://selfdestruct.net/misc/usbboot/SP27213.exe)           | HP USB Disk Storage Format Tool 2.0.6 for Windows. See [LinuxBootDiskFormat](LinuxBootDiskFormat.md) for using Linux fdisk instead |

Images can be built by compiling the sources, follow
[Compiling Procedure](http://code.google.com/p/beagleboard/wiki/BeagleSoftCompile)

To execute built Images on Beagle Board follow [Booting Procedure](http://code.google.com/p/beagleboard/wiki/BootingBeagleBoard)

[Links to Deprecated Beagle Board Sources](http://code.google.com/p/beagleboard/wiki/DeprecatedBeagleSources)