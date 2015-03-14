# Procedure to Flash NAND on Beagle Board #

## Flashing automatically while u-boot boots(using bootcmd feature) ##
  * Format MMC/SD card as mentioned in [Beagle Hardware Setup](http://code.google.com/p/beagleboard/wiki/BeagleBootHwSetup)
  * Save [x-load.bin.ift](http://beagleboard.googlecode.com/files/x-load.bin.ift_for_NAND) to be flashed onto NAND as x-load.bin.ift on MMC
  * Save [MLO for MMC booting](http://beagleboard.googlecode.com/files/MLO_revb) as MLO on MMC
  * Save [u-boot.bin to boot](http://beagleboard.googlecode.com/files/u-boot.bin_autoflash) as u-boot.bin on MMC
  * Save [u-boot to flash](http://beagleboard.googlecode.com/files/flash-uboot.bin) as flash-uboot.bin on MMC
  * [Setup the Beagle Board](http://code.google.com/p/beagleboard/wiki/BootingBeagleBoard) to boot over MMC
  * When Beagle boots it automatically flashes u-boot and x-loader onto NAND flash

NOTE:
If NAND was flashed already then it might not automatically run the bootcmd.
Erase the flash using NAND Command
```
      OMAP3 beagleboard.org # nand unlock
     OMAP3 beagleboard.org # nand erase
```
Then repeat the procedure

## Flashing commands with u-boot ##

  * Format MMC/SD card as mentioned in
  * Save [x-load.bin.ift](http://beagleboard.googlecode.com/files/x-load.bin.ift_for_NAND) to be flashed onto NAND as x-load.bin.ift on MMC
  * Save [MLO for MMC booting](http://beagleboard.googlecode.com/files/MLO_revb) as MLO on MMC
  * Save [u-boot.bin to boot](http://beagleboard.googlecode.com/files/u-boot.bin_autoflash) as u-boot.bin on MMC
  * Save [u-boot to flash](http://beagleboard.googlecode.com/files/flash-uboot.bin) as flash-uboot.bin on MMC
  * [Setup the Beagle Board](http://code.google.com/p/beagleboard/wiki/BootingBeagleBoard) to boot over MMC
  * On u-boot prompt execute the following commands
```
         OMAP3 beagleboard.org # mmcinit
        OMAP3 beagleboard.org # fatload mmc 0 0x80200000 x-load.bin.ift
        OMAP3 beagleboard.org # nand unlock
        OMAP3 beagleboard.org # nand ecc hw
        OMAP3 beagleboard.org # nand erase 0 80000
        OMAP3 beagleboard.org # nand write.i 0x80200000 0 80000
        OMAP3 beagleboard.org # fatload mmc 0 0x80200000 flash-uboot.bin
        OMAP3 beagleboard.org # nand unlock
        OMAP3 beagleboard.org # nand ecc sw
        OMAP3 beagleboard.org # nand erase 80000 160000
        OMAP3 beagleboard.org # nand write.i 0x80200000 80000 160000
```

## Flashing commands with uImage ##

```
To flash the uImage
OMAP3 beagleboard.org # mmcinit
OMAP3 beagleboard.org # fatload mmc 0 0x80300000 uImage
OMAP3 beagleboard.org # nand unlock
OMAP3 beagleboard.org # nand ecc sw
OMAP3 beagleboard.org # nand erase 280000 47FFFF
OMAP3 beagleboard.org # nand write.i 0x80300000 280000 1FFFFF
```

```
To read the uImage back to RAM and boot
OMAP3 beagleboard.org # nand read.i 0x80300000 0x280000 0x1FFFFF
OMAP3 beagleboard.org # bootm 0x80300000
```