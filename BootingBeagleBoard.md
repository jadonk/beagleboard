# Booting Beagle Software #
Beagle Board can boot from NAND, MMC, UART, and USB. At present we support only NAND and MMC booting. The compilation steps for software components and pre-built images are described at [Beagle Software Compilation Procedure](http://code.google.com/p/beagleboard/wiki/BeagleSoftCompile)

## Booting u-boot on Beagle Board ##

The u-boot booting on NAND and MMC need another software component called x-loader. The booting procedure for x-loader as such doesn't need any detailed explanation, so this section includes x-loader booting as well.

**Booting u-boot on NAND Flash
  * Assumption: All Beagle "Rev B" Boards come with pre-loaded u-boot on NAND flash
  * Do the [Hardware Setup](http://code.google.com/p/beagleboard/wiki/BeagleBootHwSetup) for booting u-boot over NAND Flash.
  * On "Powering ON" the board, u-boot 1.3.3 should boot by displaying a splash screen on DVI and playing a tone on audio out, the u-boot console should be shown as below
```
        Texas Instruments X-Loader 1.41
        Starting OS Bootloader...


        U-Boot 1.3.3 (Jul  8 2008 - 16:29:02)

        OMAP3530-GP rev 2, CPU-OPP2 L3-165MHz
        OMAP3 Beagle Board + LPDDR/NAND
        DRAM:  128 MB
        NAND:  256 MiB
        *** Warning - bad CRC or NAND, using default environment

        In:    serial
        Out:   serial
        Err:   serial
        Audio Tone on Speakers  ... complete
        Hit any key to stop autoboot:  0
        OMAP3 beagleboard.org #
```**

NOTE: If u-boot is not Flashed on the NAND, then refer to [Beagle NAND Flash Procedure](http://elinux.org/BeagleBoardNAND) to do the same

**Booting u-boot with MMC/SD Card
  * Do the [Hardware Setup](http://code.google.com/p/beagleboard/wiki/BeagleBootHwSetup) for booting u-boot over MMC/SD Card.
  * On "Powering ON" the board, u-boot 1.3.3 should boot by displaying a splash screen on DVI and playing a tone on audio out, the u-boot console should be shown as below
```
       Texas Instruments X-Loader 1.41
       Starting on with MMC
       Reading boot sector

       717372 Bytes Read from MMC
       Starting OS Bootloader from MMC...


       U-Boot 1.3.3 (Jul  8 2008 - 19:29:48)

       OMAP3530-GP rev 2, CPU-OPP2 L3-165MHz
       OMAP3 Beagle Board + LPDDR/NAND
       DRAM:  128 MB
       NAND:  256 MiB
       In:    serial
       Out:   serial
       Err:   serial
       Audio Tone on Speakers  ... complete
       Hit any key to stop autoboot:  0
       OMAP3 beagleboard.org #
```**

## Boot the Linux Image on Beagle Board ##

Once you have u-boot booted over NAND or MMC, a Linux kernel image can be booted on Beagle Board.

The Linux Kernel Image (uImage) can be downloaded on to Beagle DDR memory using

UART (time consuming), MMC, NAND (if it was stored in it), USB (Not supported yet).

The Below procedure gives MMC based Linux Kernel Booting.

  1. Complile the Linux Kernel Image "uImage".
  1. Copy the uImage file in MMC/SD card pre-formated for FAT32.
  1. Download the uImage
```
       OMAP3 beagleboard.org # mmcinit
       OMAP3 beagleboard.org # fatload mmc 0 0x80300000 uImage
```
  1. Set/Configure the boot arguments
The filesystem to be mounted could be present in MMC, RAM (Ramdisk), NAND (if copied), Ethernet (using Ethernet over USB Dongle on USB HOST machine). The Bootargs for each of these is shown Below
    * Bootargs for RAMDISK File System
```
          OMAP3 beagleboard.org # setenv bootargs console=ttyS2,115200n8 ramdisk_size=8192 root=/dev/ram0 rw rootfstype=ext2 initrd=0x81600000,8M nohz=off
```
    * Bootargs for MMC File System
```
          OMAP3 beagleboard.org # setenv bootargs console=ttyS2,115200n8 noinitrd root=/dev/mmcblk0p1 rootfstype=ext2 rw rootdelay=1 nohz=off
```
    * Bootargs for NAND (JFFS2) File System)
> > > TBD
    * Bootargs for NFS (using Ethernet over USB Dongle)
> > > TBD
  1. Getting File System on Beagle Board
    * RAMDISK File system
```
          OMAP3 beagleboard.org # fatload mmc 0 0x81600000 rd-ext2.bin
  
          NOTE: rd-ext2.bin should have been copied onto MMC Card.
```
    * MMC File system
      * Copy Filesystem on MMC/SD card.
```
          Format an MMC/SD card for ext2/ext3 file system using Linux Machine
          Mount the MMC/SD card on Host Linux Machine
          UnTar the Pre-built Filesystem
          Un-Mount the MMC/SD card on Host Linux Machine
```
      * Remove the MMC/SD card that had uImage, and insert the MMC/SD card that has Filesystem.
  1. Booting the Kernel Image
```
       OMAP3 beagleboard.org # bootm 0x80300000
```