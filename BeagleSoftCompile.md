# Beagle Board Software Compilation Options and Procedure #

Related Links:

[Beagle Source and Tools](http://code.google.com/p/beagleboard/wiki/BeagleSourceCode)

[Beagle Software Booting Procedure](http://code.google.com/p/beagleboard/wiki/BootingBeagleBoard)

# Compiling x-loader #

**Compiling x-loader for NAND booting
  * In file**_include/configs/omap3530beagle.h_*** Disable the "CFG\_CMD\_MMC" Macro
```
       /* For X-loader to be flashed on to NAND disable the below macro */
       //#define CFG_CMD_MMC              1
```
  * Comiple the x-loader as shown below
```
       make CROSS_COMPILE=arm-none-linux-gnueabi- distclean
      make CROSS_COMPILE=arm-none-linux-gnueabi- omap3530beagle_config
      make CROSS_COMPILE=arm-none-linux-gnueabi-
```
> > File named "x-load.bin" will be generated
  * Convert x-load.bin to x-load.bin.ift (required to FLASH x-loader to NAND)
    1. Use the "SignGP" tool to sign the x-loader image.
```
       ./signGP x-load.bin
```
    1. Copy x-load.bin.ift to MMC/SD card using a card reader/writer or download it through UART.
> > _[Prebuilt Image](http://beagleboard.googlecode.com/files/x-load.bin.ift_for_NAND)_ for testing (Save this as x-load.bin.ift)**

**Compiling x-loader for MMC booting
  * In file**_include/configs/omap3530beagle.h_*** Enable the "CFG\_CMD\_MMC" Macro
```
       /* For X-loader to be flashed on to NAND disable the below macro */
       #define CFG_CMD_MMC              1
```
  * Comiple the x-loader as shown below
```
       make CROSS_COMPILE=arm-none-linux-gnueabi- distclean
      make CROSS_COMPILE=arm-none-linux-gnueabi- omap3530beagle_config
      make CROSS_COMPILE=arm-none-linux-gnueabi-
```
> > File named "x-load.bin" will be generated
  * Convert x-load.bin to MLO (required for MMC booting)
    1. Use the "SignGP" tool to sign the x-loader image.
```
         ./signGP x-load.bin              
```
    1. Rename x-load.bin.ift to MLO
    1. Copy MLO to MMC/SD card using a card reader/writer.
> > _[Prebuilt Image](http://beagleboard.googlecode.com/files/MLO_revb)_ for testing (Save this as MLO)**

# Compiling u-boot #

**Compiling u-boot for Flashing NAND automatically
  * In file**_include/configs/omap3530beagle.h_*** Enable the CONFIG\_BOOTCOMMAND Macro as shown below
```
       Un comment the below CONFIG_BOOTCMD 
       
       #define CONFIG_BOOTCOMMAND       \
        "mmcinit;fatload mmc 0 0x80200000 x-load.bin.ift;\
        nand unlock;nand ecc hw;nand erase 0 80000;nand write.i 0x80200000 0 80000;\
        fatload mmc 0 0x80200000 flash-uboot.bin; nand unlock;\
        nand ecc sw;nand erase 80000 160000; nand write.i 0x80200000 80000 160000;\0"
       
       Comment the below line as shown below

       /* #define CONFIG_BOOTCOMMAND "\0" */

```
  * Comiple the u-boot as shown below
```
       make CROSS_COMPILE=arm-none-linux-gnueabi- distclean
      make CROSS_COMPILE=arm-none-linux-gnueabi- omap3530beagle_config
      make CROSS_COMPILE=arm-none-linux-gnueabi-
```
> > File named "u-boot.bin" will be generated**


> _[Prebuilt Image](http://beagleboard.googlecode.com/files/u-boot.bin_autoflash)_ for testing (Save this as u-boot.bin)

**Compiling u-boot for regular Kernel Booting
  * In file**_include/configs/omap3530beagle.h_*** Enable the CONFIG\_BOOTCOMMAND Macro as shown below
```
       Comment the below CONFIG_BOOTCOMMAND macro

       /*
       #define CONFIG_BOOTCOMMAND       \
        "mmcinit;fatload mmc 0 0x80200000 x-load.bin.ift;\
        nand unlock;nand ecc hw;nand erase 0 80000;nand write.i 0x80200000 0 80000;\
        fatload mmc 0 0x80200000 flash-uboot.bin; nand unlock;\
        nand ecc sw;nand erase 80000 160000; nand write.i 0x80200000 80000 160000;\0"
       */

       Un-comment CONFIG_BOOTCOMMAND macro as shown below
       #define CONFIG_BOOTCOMMAND "\0" 
```
  * Comiple the u-boot as shown below
```
       make CROSS_COMPILE=arm-none-linux-gnueabi- distclean
      make CROSS_COMPILE=arm-none-linux-gnueabi- omap3530beagle_config
      make CROSS_COMPILE=arm-none-linux-gnueabi-
```
> > File named "u-boot.bin" will be generated**


> _[Prebuilt Image](http://beagleboard.googlecode.com/files/flash-uboot.bin)_ for testing (Save this as u-boot.bin for booting over MMC)
> (Save the same as flash-uboot.bin in MMC for flashing automatically to NAND)

# Compiling Kernel #

  * Compile the Kernel as shown below
```
       make CROSS_COMPILE=arm-none-linux-gnueabi- distclean
      make CROSS_COMPILE=arm-none-linux-gnueabi- omap3_beagle_defconfig
      make CROSS_COMPILE=arm-none-linux-gnueabi- uImage             
```
> > File named "uImage" will be generated in arch/arm/boot directory


> _[Prebuilt Kernel Image](http://beagleboard.googlecode.com/files/uImage_OTG) for testing_ (Save this as uImage)