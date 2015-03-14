#Beagle Board Diagnostic Tools and Procedure

# What's this all about? #

This page discusses the process of verifying the functionality of your BeagleBoard Revision Bx/Cx or BeagleBoard-xM Revision Ax. In essence, it describes the steps you might want to take after unwrapping a brand new board, holding it up (carefully, by its edges, of course) and thinking, "Um, OK, now what?"  This process will set any non-volatile memory on your BeagleBoard to factory-release condition.

While there is more than one way to power up and connect to your board, these instructions assume you will use a 5V power supply, and connect to the standard serial port via a USB/serial adapter using minicom.

[Project launch e-mail thread](http://groups.google.com/group/beagleboard/browse_thread/thread/ecaeace7577092e3)

# Rebuilding the validation and demo images #

If all you want to do is test your board, please skip this section and use the SD card image provided for you in [the next section](#The_setup.md).  This section just talks about how to recreate work that has already been done for you.

A single script, [ec2build.sh](http://beagleboard-validation.s3.amazonaws.com/deploy/201008201549/sd/ec2build.sh), was used to build the validation and demo images and points to all the necessary source code repositories.  You can always find the [latest version under version control on gitorious.org](http://gitorious.org/beagleboard-validation/scripts/blobs/master/ec2build.sh).  This script utilizes [Amazon Elastic Compute Cloud (EC2)](http://aws.amazon.com/ec2/).  It is also possible to utilize this script on your own machine running Ubuntu 10.04.

  1. Have a machine running Linux.
  1. Install the [Amazon EC2 API Tools](http://developer.amazonwebservices.com/connect/entry.jspa?externalID=351&categoryID=88).
  1. Sign up for [Amazon EC2](http://aws.amazon.com/ec2).
  1. Sign up for [Amazon S3](http://aws.amazon.com/s3).
  1. wget http://beagleboard-validation.s3.amazonaws.com/deploy/201008201549/sd/ec2build.sh
  1. Create ~/secret/setup\_env.sh with required environment variables specified in ec2build.sh.
  1. ./ec2build.sh run-build

If you are using your own machine with Ubuntu 10.04 on it, below are the steps to build.

  1. wget http://beagleboard-validation.s3.amazonaws.com/deploy/201008201549/sd/ec2build.sh
  1. ./ec2build.sh setup-oe
  1. Download [TI C6000 CGT tools](https://focus-webapps.ti.com/licreg/docs/swlicexportcontrol.tsp?form_type=2&prod_no=ti_cgt_c6000_6.1.9_setup_linux_x86.bin&ref_url=http://software-dl.ti.com/dsps/dsps_public_sw/sdo_sb/targetcontent/omap_l138/1_00/latest/) to /mnt/angstrom-setup-scripts/sources/downloads.
  1. Download the [Graphics SDK](https://focus-webapps.ti.com/licreg/docs/swlicexportcontrol.tsp?form_type=2&prod_no=OMAP35x_Graphics_SDK_setuplinux_3_01_00_06.bin&ref_url=http://software-dl.ti.com/dsps/dsps_public_sw/gfxsdk/3_01_00_06/) to /mnt/angstrom-setup-scripts/sources/downloads.
  1. ./ec2build.sh build-image test
  1. Edit the mount-s3 routine in ec2build.sh to create a /mnt/s3 folder you can write to.
  1. ./ec2build.sh build-sd test
  1. ./ec2build.sh build-image demo
  1. ./ec2build.sh build-sd demo

# The setup #

## Preparing the validation SD card ##

If you are using a BeagleBoard-xM, you can skip this section and go to [board and peripheral setup](#Board_and_peripheral_setup.md), unless you need to reprogram your microSD card or create a backup copy.

For these validation tests, you'll need a 128MB or greater SD card (or microSD card for the BeagleBoard-xM) that you can completely overwrite (all contents will be lost).

To initialize your card under Windows, you can do the following:

  1. Download and install [Ubuntu's Win32DiskImager](https://wiki.ubuntu.com/Win32DiskImager) (also known as the [win32-image-writer](https://launchpad.net/win32-image-writer/)).
  1. Download and install [7-zip compression software](http://www.7-zip.org).
  1. Download the SD card image you want to use. The [BeagleBoard validation SD card image](http://beagleboard-validation.s3.amazonaws.com/deploy/201008201549/sd/beagleboard-validation-201008201549.img.gz) is a simple ramdisk-based console image and doesn't include anything in the second partition, allowing you to repartition the disk and put whatever you want on that second partition, and ships with xM boards up to at least revision B1.  The [demo image](http://beagleboard-validation.s3.amazonaws.com/deploy/201008201549/sd/beagleboard-demo-201008201549-configured.img.gz) image is much bigger and is a full GUI image with lots of demos.
  1. Decompress the verification image file using 7-zip.
  1. Insert the SD card writer/reader into the Windows machine.
  1. Insert >=128MB SD card into the reader/writer. (If you are using [the demo image file](http://beagleboard-validation.s3.amazonaws.com/deploy/201008201549/sd/beagleboard-demo-201008201549-configured.img.gz) you will need an SD card of 4GB or greater)
  1. Start the Win32DiskImager.
  1. Select the decompressed verification image file and correct SD card location.
  1. Click on 'Write'.
  1. After the image writing is done, eject the SD card.

If you're doing this under Linux, then just:

> For validation image:
  1. wget http://beagleboard-validation.s3.amazonaws.com/deploy/201008201549/sd/beagleboard-validation-201008201549.img.gz
  1. zcat beagleboard-validation-0201008201549.img.gz | dd of=/dev/your/sd/card bs=8225280

> For demo image:
  1. wget http://beagleboard-validation.s3.amazonaws.com/deploy/201008201549/sd/beagleboard-demo-201008201549-configured.img.gz
  1. zcat beagleboard-demo-201008201549-configured.img.gz | dd of=/dev/your/sd/card bs=8225280

## Board and peripheral setup ##

Depending on what you want to test and what peripherals you have at hand, do as much of the following as possible.

  1. Make sure your BeagleBoard is not connected to power.
  1. Connect UART cable to a UART port of Window/Linux/Mac machine.  For your revision Bx/Cx BeagleBoard, a 10-pin IDC adapter and null-modem cable should be used.  For your revision xM-Ax BeagleBoard-xM, use of a USB-to-serial adapter or straight-through cable should be used.
  1. Have terminal program, such as [TeraTerm for Windows](http://en.wikipedia.org/wiki/TeraTerm), HyperTerminal for Windows, [Minicom for Linux](http://en.wikipedia.org/wiki/Minicom), or [screen for Linux/Mac](http://embeddedfreak.wordpress.com/2008/08/12/using-gnu-screen-to-debug-your-serial-port/) running on the host machine.
  1. Configure the terminal program for (BAUD RATE - 115200, DATA - 8 bit, PARITY- none, STOP - 1bit, FLOW CONTROL - none)
  1. Insert the SD card (that is prepared as described above) into SD slot on your BeagleBoard.
  1. Connect a LCD monitor to the DVI-D port (HDMI connector) on your BeagleBoard.
  1. Connect an externally powered speaker to the Audio Out jack your BeagleBoard.
  1. Connect a line-in cable from an audio source (or y-cable from your BeagleBoard Audio Out) to the Audio In jack on your BeagleBoard.
  1. Connect a TV (NTSC-M) to the S-video port on your BeagleBoard.
  1. Power ON your LCD monitor, TV and audio speakers.
  1. Connect one end of USB mini B to USB A cable to your BeagleBoard. **Do NOT** connect the USB A side to the host machine yet.
  1. If you have Windows PC as a host machine, then copy the [Linux.inf](http://beagleboard.googlecode.com/files/linux.inf) RNDIS driver configuration file to the host machine. Generally Windows will have the driver.
  1. Also for Windows PCs, copy the [Gserial.inf](http://beagleboard.googlecode.com/files/gserial.inf) USB serial port driver configuration to the host machine.  Generally Windows will have the driver.

## Preparing the flash on your BeagleBoard ##

If you are using a BeagleBoard-xM, you can skip this section since they do not have NAND flash.

The individual MLO and u-boot.bin files can be acquired from http://beagleboard-validation.s3.amazonaws.com/deploy/201008201549/sd/list.html, but you can just use the SD card configured above.

Halt the u-boot process and input these commands before continuing.

```
mmcinit
mmc init
fatload mmc 0 82000000 MLO
nand unlock
nand ecc hw
nandecc hw
nand erase 0 80000
nand write 82000000 0 20000
nand write 82000000 20000 20000
nand write 82000000 40000 20000
nand write 82000000 60000 20000
fatload mmc 0 0x80200000 u-boot.bin
nand unlock
nand ecc sw
nandecc sw
nand erase 80000 160000
nand write 0x80200000 80000 160000
nand erase 260000 20000
```

# Validation at the U-Boot level #

Regardless of what OS you plan on booting into, you can interrupt the boot process to stop in the U-Boot loader and take a look around to verify that everything looks sane, at least with respect to U-Boot.  If you're interested in what's happening at the U-Boot level, here are a few things you can check.

## boot ##
Plug-in the recommended 5V power supply into your BeagleBoard and observe the output on the terminal program.  When the count-down starts, press ENTER:

xM Rev Ax (Micron):
```


Texas Instruments X-Loader 1.4.4ss (Aug 19 2010 - 02:49:27)
Beagle xM Rev A
Reading boot sector
Loading u-boot.bin from mmc


U-Boot 2010.03-dirty (Aug 20 2010 - 20:50:46)

OMAP3630/3730-GP ES1.0, CPU-OPP2, L3-165MHz, 
OMAP3 Beagle board + LPDDR/NAND
I2C:   ready
DRAM:  512 MB
NAND:  0 MiB
*** Warning - bad CRC or NAND, using default environment

In:    serial
Out:   serial
Err:   serial

Probing for expansion boards, if none are connected you'll see a harmless I2C error.

No EEPROM on expansion board
Beagle xM Rev A
Die ID #54da0000061000000156166b0a03200d
Hit any key to stop autoboot:  0 
OMAP3 beagleboard.org # 
```

xM Rev Ax (Numonyx) or Beagle Rev C5 is the same except for:
```
NAND:  256 MiB
```

For a BeagleBoard Rev C4, the difference is see:
```
NAND:  128 MiB
```

At least one person reported an issue with the "Texas Instruments X-Loader" being repeatedly displayed when powering the board via the OTG port.  Switching to the recommended USB Y-cable resolved the issue.

The NAND on the xM Rev Ax boards isn't supported and the Numonyx memories with the NAND in them won't be readily available.  Use at your own peril.

The Die ID # is different for every board.

If for some reason you get something without the "**Warning - bad CRC or NAND, using default environment" notice, then it is likley that you have NAND flash and you've executed 'setenv'.  To get back to a working state, simply erase the NAND flash:**

```
OMAP3630/3730-GP ES1.0, CPU-OPP2, L3-165MHz,
OMAP3 Beagle board + LPDDR/NAND
I2C:   ready
DRAM:  512 MB
NAND:  256 MiB
In:    serial
Out:   serial
Err:   serial

Probing for expansion boards, if none are connected you'll see a harmless I2C error.

No EEPROM on expansion board
Beagle xM Rev A
Die ID #52a40000061000000156166b0a01400c
Hit any key to stop autoboot:  0
OMAP3 beagleboard.org # nand erase

NAND erase: device 0 whole chip
Skipping bad block at  0x04b80000
Skipping bad block at  0x05de0000
Skipping bad block at  0x0d5a0000
Erasing at 0xffe0000 -- 100% complete.
OK
OMAP3 beagleboard.org #
```

## version ##

First, there's the version of U-Boot you're loading (corresponding to the current version that's downloadable from the Angstrom distribution):

```
OMAP3 beagleboard.org # version


U-Boot 2010.03-dirty (Aug 20 2010 - 20:50:46)
OMAP3 beagleboard.org # 
```

## bdinfo ##

Next, you can examine the board information:

xM Rev Ax (Micron):
```
OMAP3 beagleboard.org # bdinfo
arch_number = 0x0000060A
env_t       = 0x00000000
boot_params = 0x80000100
DRAM bank   = 0x00000000
-> start    = 0x80000000
-> size     = 0x10000000
DRAM bank   = 0x00000001
-> start    = 0x90000000
-> size     = 0x10000000
baudrate    = 115200 bps
OMAP3 beagleboard.org #
```

xM Rev Ax (Numonyx):
```
OMAP3 beagleboard.org # bdinfo
arch_number = 0x0000060A
env_t       = 0x00000000
boot_params = 0x80000100
DRAM bank   = 0x00000000
-> start    = 0x80000000
-> size     = 0x20000000
DRAM bank   = 0x00000001
-> start    = 0xA0000000
-> size     = 0x00000000
baudrate    = 115200 bps
OMAP3 beagleboard.org #
```

xM Rev Cx (Micron):
```
arch_number = 0x0000060A
boot_params = 0x80000100
DRAM bank   = 0x00000000
-> start    = 0x80000000
-> size     = 0x10000000
DRAM bank   = 0x00000001
-> start    = 0x90000000
-> size     = 0x10000000
ethaddr     = (not set)
ip_addr     = 0.0.0.0
baudrate    = 115200 bps
TLB addr    = 0x9FFF0000
relocaddr   = 0x9FF65000
reloc off   = 0x1FF5D000
irq_sp      = 0x9FF04F68
sp start    = 0x9FF04F58
FB base     = 0x00000000
```

The RAM bank information should be self-explanatory, while the architecture number represents the official ARM designation of this particular board, as listed [here](http://www.arm.linux.org.uk/developer/machines/).  Converting hex '60A' to decimal gives 1546, which you can see is, in fact, the BeagleBoard in that list.  If that arch\_number value shows anything other than 0x60A, something has gone wrong.

You'll notice on the xM Rev Ax boards that ones with the Micron memory have 2 banks and ones with Numonyx memory have just 1 bank.  Both provide the 512MB of RAM required for xM boards.

## coninfo ##

Next, print out the console information:

xM Rev Ax:
```
OMAP3 beagleboard.org # coninfo
List of available devices:
serial   80000003 SIO stdin stdout stderr
usbtty   00000003 .IO
OMAP3 beagleboard.org # 
```

## nand ##

List the NAND flash information with:

BeagleBoard-xM Rev Ax (Micron), BeagleBoard-xM Rev Bx, BeagleBoard-xM Rev Cx:
```
OMAP3 beagleboard.org # nand info

OMAP3 beagleboard.org #
```

BeagleBoard-xM Rev Ax (Numonyx), BeagleBoard C4/C5:
```
OMAP3 beagleboard.org # nand info

Device 0: nand0, sector size 128 KiB
OMAP3 beagleboard.org #
```

Keep in mind that NAND is not supported on the xM Rev Ax boards.

Curiously, the older version of U-Boot was slightly more informative:

```
OMAP3 beagleboard.org # nand info                                               
                                                                                
Device 0: NAND 256MiB 1,8V 16-bit, sector size 128 KiB                          
OMAP3 beagleboard.org #
```

How odd.

## mtdparts ##

You can see the partition breakdown of flash with:

BeagleBoard-xM Rev Ax (Micron), BeagleBoard-xM Rev Bx, BeagleBoard-xM Rev Cx :
```
OMAP3 beagleboard.org # mtdparts 
mtdparts variable not set, see 'help mtdparts'
Device nand0 not found!
```

BeagleBoard-xM Rev Ax (Numonyx), BeagleBoard C4/C5:
```
OMAP3 beagleboard.org # mtdparts
mtdparts variable not set, see 'help mtdparts'
no partitions defined

defaults:
mtdids  : nand0=nand
mtdparts: mtdparts=nand:512k(x-loader),1920k(u-boot),128k(u-boot-env),4m(kernel),-(fs)
OMAP3 beagleboard.org #
```

Again, remember that the NAND is not supported on the xM Rev Ax boards.

See the output of `help mtdparts` to see what else you can do here in terms of setting environment variables.

## i2c ##

The `i2c` command has a number of subcommands, but to simply see the I2C information, run:

```
OMAP3 beagleboard.org # i2c probe
Valid chip addresses: 48 49 4A 4B
Excluded chip addresses: 00
OMAP3 beagleboard.org # i2c dev 2
Setting bus to 2
OMAP3 beagleboard.org # i2c probe
Valid chip addresses: 37 3A 50
Excluded chip addresses:
OMAP3 beagleboard.org # i2c md 50 0 80
0000: 00 ff ff ff ff ff ff 00 1e 6d 4a 4b fe 95 00 00    .........mJK....
0010: 04 11 01 03 ea 22 1b 78 ea 32 31 a3 57 4c 9d 25    .....".x.21.WL.%
0020: 11 50 54 a5 6a 80 31 4f 45 4f 61 4f 81 80 01 01    .PT.j.1OEOaO....
0030: 01 01 01 01 01 01 30 2a 00 98 51 00 2a 40 30 70    ......0*..Q.*@0p
0040: 13 00 52 0e 11 00 00 1e 00 00 00 fd 00 38 4b 1e    ..R..........8K.
0050: 47 0b 00 0a 20 20 20 20 20 20 00 00 00 fc 00 4c    G...      .....L
0060: 31 39 33 33 54 52 0a 20 20 20 20 20 00 00 00 fc    1933TR.     ....
0070: 00 20 0a 20 20 20 20 20 20 20 20 20 20 20 00 9e    . .           ..
OMAP3 beagleboard.org # i2c dev 0
Setting bus to 0
OMAP3 beagleboard.org #
```

In the above, I've dumped the EDID of the connected monitor.  The EDID is simply an I2C EEPROM in the monitor.  Note that my monitor is an LG model L1933TR.  The format of the EDID data is described in the [Wikipedia article on Extended display identification data](http://en.wikipedia.org/wiki/Extended_display_identification_data):

  * The first 8 bytes are just a header.
  * Manufacturer ID: 1e 6d = 0 00111 10011 01101 = GSM = Goldstar Company Ltd per [MSFT spreadsheet](http://download.microsoft.com/download/7/E/7/7E7662CF-CBEA-470B-A97E-CE7CE0D98DC2/ISA_PNPID_List.xlsx)
  * Product ID: 0x4b4a
  * Serial number: 0x000095fe
  * Week of manufacture: 4
  * Year of manufacture: 2007
  * EDID version/revision: 1.3
  * Video input definition: 11101010 =
    * Input: digital
    * Video level: 0.7, 0
    * Blank-to-black setup: no
    * Separate syncs: yes
    * Composite sync: no
    * Sync on green: yes
    * Serration vsync: no
  * Maximum horizontal image size: 34cm (13.4")
  * Maximum vertical image size: 27cm (10.6")
  * Display gamma: 2.2
  * Power management and supported features: 11101010 =
    * Standby: yes
    * Suspend: yes
    * Active-off/low power: yes
    * Display type: RGB color
    * Standard color space: no
    * Preferred timing mode: yes
    * Default GTF supported: no
  * Chromatic coordinates: 32 31 a3 57 4c 9d 25 11 50 54
  * Established timings supported: 10100101 01101010 10000000 =
    * 720x400@70Hz, 640x480@60Hz, 640x480@75Hz, 800x600@60Hz
    * 800x600@75Hz, 832x624@75Hz, 1024x768@60Hz, 1024x768@75Hz
    * 1152x870@75Hz
  * Standard timings: 31 4f, 45 4f, 61 4f, 81 80, 01 01, 01 01, 01 01, 01 01 =
    * 640x480@75Hz
    * 800x600@75Hz
    * 1024x768@75Hz
    * 1280x1024@60Hz

## mmc ##

To initialize your SD card, run:

```
OMAP3 beagleboard.org # mmc init
mmc1 is available
OMAP3 beagleboard.org # 
```

If you didn't set the I2C bus back to 0, you might get the following error:

```
OMAP3 beagleboard.org # mmc init
I2C read: I/O error
I2C read: I/O error
mmc1 is available
OMAP3 beagleboard.org # 
```

Once that's done, you can examine the contents of your FAT partition:

```
OMAP3 beagleboard.org # fatls mmc 1
    24296   mlo
   210360   u-boot.bin
  3190568   uimage
 19960110   ramdisk.gz
      755   user.scr
 19509297   ramfs.img
      376   md5sum.txt

7 file(s), 0 dir(s)

OMAP3 beagleboard.org # fatload mmc 1 80200000 user.scr
reading user.scr

755 bytes read
OMAP3 beagleboard.org #
```

## mtest ##

If you want to read/write test your RAM:

```
OMAP3 beagleboard.org # help mtest
mtest - simple RAM read/write test

Usage:
mtest [start [end [pattern [iterations]]]]
OMAP3 beagleboard.org # mtest 0x82000000 0x82FFFFFF 0xdeadbeef 1
Testing 82000000 ... 82ffffff:
Tested 1 iteration(s) with 0 errors.
OMAP3 beagleboard.org #
```

## led ##

The code isn't yet upstream, but we have added support for the LEDs:

```
OMAP3 beagleboard.org # help led
led - led       - [0|1|green|all] [on|off]


Usage:
led led [led_name] [on|off] sets or clears led(s)

OMAP3 beagleboard.org # led all off
OMAP3 beagleboard.org # led all on
OMAP3 beagleboard.org # led 0 off
OMAP3 beagleboard.org # led 0 on
OMAP3 beagleboard.org # led 1 off
OMAP3 beagleboard.org # led 1 on
OMAP3 beagleboard.org #
```


## userbutton ##

There is also a hack for checking the status of the USER button:

```
OMAP3 beagleboard.org # userbutton
The user button is currently NOT pressed.
OMAP3 beagleboard.org #
```

Press the button and execute the command again:

```
OMAP3 beagleboard.org # userbutton
The user button is currently PRESSED.
OMAP3 beagleboard.org #
```

## usbtty ##

_THIS TEST IS NOT YET VALID_

There is a mistake in the u-boot code in that it is using TI's VID (0x0451).  I needed to modify the PID in gserial.inf to make it match.

```

```

usbser.sys is in C:\WINDOWS\system32\drivers on Windows XP.

# Booting into Linux #

This covers booting into the validation Linux image, but some of the lessons learned here should also be beneficial for understanding booting into more full featured Linux distributions.

If you are using the validation SD card on a BeagleBoard-xM, then you can simply disconnect, wait a second, and reapply power to perform boot (given that you've performed the setup above).  Using either the demo or validation images on a BeagleBoard-xM, uppon applying power, you can wait for D14 to turn on and then off again (or u-boot shows the 3 second count-down), then hold the USER button for 3 seconds to be sure that boot.scr is ignored and user.scr is used:

xM Rev Ax (Numonyx):
```
Texas Instruments X-Loader 1.4.4ss (Aug 19 2010 - 02:49:27)
Beagle xM Rev A
Reading boot sector
Loading u-boot.bin from mmc


U-Boot 2010.03-dirty (Aug 20 2010 - 20:50:46)

OMAP3630/3730-GP ES1.0, CPU-OPP2, L3-165MHz, 
OMAP3 Beagle board + LPDDR/NAND
I2C:   ready
DRAM:  512 MB
NAND:  256 MiB
*** Warning - bad CRC or NAND, using default environment

In:    serial
Out:   serial
Err:   serial

Probing for expansion boards, if none are connected you'll see a harmless I2C error.

No EEPROM on expansion board
Beagle xM Rev A
Die ID #52a40000061000000156166b0a01400c
Hit any key to stop autoboot:  0 
mmc1 is available
The user button is currently PRESSED.
reading user.scr

755 bytes read
Running bootscript from mmc ...
## Executing script at 80200000
mmc1 is available
reading ramdisk.gz

19960110 bytes read
reading uImage

3190568 bytes read
Booting from ramdisk ...
## Booting kernel from Legacy Image at 80200000 ...
   Image Name:   Angstrom/2.6.32/beagleboard
   Image Type:   ARM Linux Kernel Image (uncompressed)
   Data Size:    3190504 Bytes =  3 MB
   Load Address: 80008000
   Entry Point:  80008000
   Verifying Checksum ... OK
   Loading Kernel Image ... OK
OK

Starting kernel ...

Uncompressing Linux................................................................................................................................................................................................................ done, booting the kernel.
[    0.000000] Linux version 2.6.32 (ubuntu@ip-10-204-115-71) (gcc version 4.3.3 (GCC) ) #3 PREEMPT Wed Aug 18 15:53:03 UTC 2010
[    0.000000] CPU: ARMv7 Processor [413fc082] revision 2 (ARMv7), cr=10c53c7f
[    0.000000] CPU: VIPT nonaliasing data cache, VIPT nonaliasing instruction cache
[    0.000000] Machine: OMAP3 Beagle Board
[    0.000000] Memory policy: ECC disabled, Data cache writeback
[    0.000000] OMAP3630/DM3730 ES1.0 (l2cache iva sgx neon isp 192mhz_clk )
[    0.000000] SRAM: Mapped pa 0x40200000 to va 0xfe400000 size: 0x100000
[    0.000000] Reserving 16777216 bytes SDRAM for VRAM
[    0.000000] Built 1 zonelists in Zone order, mobility grouping on.  Total pages: 117760
[    0.000000] Kernel command line: console=tty0 console=ttyS2,115200n8 mem=80M@0x80000000 mem=384M@0x88000000 mpurate=1000 buddy=none camera=lbcm3m1 vram=16M omapfb.vram=0:8M,1:4M,2:4M omapfb.mode=dvi:1024x768MR-16@60 omapdss.def_disp=dvi root=/dev/ram0 rw ramdisk_size=131072 initrd=0x88000000,128M rootfstype=ext2
[    0.000000] Beagle expansionboard: none
[    0.000000] Beagle cameraboard: lbcm3m1
[    0.000000] PID hash table entries: 2048 (order: 1, 8192 bytes)
[    0.000000] Dentry cache hash table entries: 65536 (order: 6, 262144 bytes)
[    0.000000] Inode-cache hash table entries: 32768 (order: 5, 131072 bytes)
[    0.000000] Memory: 80MB 384MB = 464MB total
[    0.000000] Memory: 316288KB available (5880K code, 671K data, 204K init, 0K highmem)
[    0.000000] Hierarchical RCU implementation.
[    0.000000] NR_IRQS:402
[    0.000000] Clocking rate (Crystal/Core/MPU): 26.0/332/600 MHz
[    0.000000] Reprogramming SDRC clock to 332000000 Hz
[    0.000000] GPMC revision 5.0
[    0.000000] IRQ: Found an INTC at 0xfa200000 (revision 4.0) with 96 interrupts
[    0.000000] Total of 96 interrupts on 1 active controller
[    0.000000] OMAP GPIO hardware version 2.5
[    0.000000] OMAP clockevent source: GPTIMER12 at 32768 Hz
[    0.000000] Console: colour dummy device 80x30
[    0.000000] console [tty0] enabled
[    0.000000] Calibrating delay loop... 532.53 BogoMIPS (lpj=2080768)
[    0.000000] Mount-cache hash table entries: 512
[    0.000000] CPU: Testing write buffer coherency: ok
[    0.000000] tmpfs: No value for mount option 'mode'
[    0.000000] devtmpfs: initialized
[    0.000000] regulator: core version 0.5
[    0.000000] NET: Registered protocol family 16
[    0.000000] Beagle cameraboard: registering i2c2 bus for lbcm3m1
[    0.000000] Found NAND on CS0
[    0.000000] Registering NAND on CS0
[    0.000000] Unable to get DVI reset GPIO
[    0.000000] omap_init_mbox: platform not supported
[    0.000000] Target VDD1 OPP = 4, VDD2 OPP = 2
[   42.385070] OMAP DMA hardware revision 5.0
[   42.391387] bio: create slab <bio-0> at 0
[   42.392669] SCSI subsystem initialized
[   42.394012] usbcore: registered new interface driver usbfs
[   42.394195] usbcore: registered new interface driver hub
[   42.394378] usbcore: registered new device driver usb
[   42.394744] i2c_omap i2c_omap.1: bus 1 rev4.0 at 2600 kHz
[   42.397521] twl4030: PIH (irq 7) chaining IRQs 368..375
[   42.397583] twl4030: power (irq 373) chaining IRQs 376..383
[   42.397888] twl4030: gpio (irq 368) chaining IRQs 384..401
[   42.399597] regulator: VUSB1V5: 1500 mV normal standby
[   42.399841] regulator: VUSB1V8: 1800 mV normal standby
[   42.400085] regulator: VUSB3V1: 3100 mV normal standby
[   42.401367] twl4030_usb twl4030_usb: Initialized TWL4030 USB module
[   42.401763] regulator: VMMC1: 1850 <--> 3150 mV normal standby
[   42.402130] regulator: VDAC: 1800 mV normal standby
[   42.402374] regulator: VPLL2: 1800 mV normal standby
[   42.402618] regulator: VSIM: 1800 <--> 3000 mV normal standby
[   42.402984] regulator: VAUX3: 1800 mV normal standby
[   42.403381] regulator: VAUX4: 1800 mV normal standby
[   42.403564] i2c_omap i2c_omap.2: bus 2 rev4.0 at 400 kHz
[   42.415832] i2c_omap i2c_omap.3: bus 3 rev4.0 at 100 kHz
[   42.417083] Switching to clocksource 32k_counter
[   42.426086] musb_hdrc: version 6.0, musb-dma, otg (peripheral+host), debug=0
[   42.429992] musb_hdrc: USB OTG mode controller at fa0ab000 using DMA, IRQ 92
[   42.430053] musb_hdrc musb_hdrc: MUSB HDRC host driver
[   42.430175] musb_hdrc musb_hdrc: new USB bus registered, assigned bus number 1
[   42.430358] usb usb1: New USB device found, idVendor=1d6b, idProduct=0002
[   42.430389] usb usb1: New USB device strings: Mfr=3, Product=2, SerialNumber=1
[   42.430450] usb usb1: Product: MUSB HDRC host driver
[   42.430480] usb usb1: Manufacturer: Linux 2.6.32 musb-hcd
[   42.430511] usb usb1: SerialNumber: musb_hdrc
[   42.431152] hub 1-0:1.0: USB hub found
[   42.431213] hub 1-0:1.0: 1 port detected
[   42.432281] NET: Registered protocol family 2
[   42.432556] IP route cache hash table entries: 4096 (order: 2, 16384 bytes)
[   42.433197] TCP established hash table entries: 16384 (order: 5, 131072 bytes)
[   42.433624] TCP bind hash table entries: 16384 (order: 4, 65536 bytes)
[   42.433837] TCP: Hash tables configured (established 16384 bind 16384)
[   42.433898] TCP reno registered
[   42.433929] UDP hash table entries: 256 (order: 0, 4096 bytes)
[   42.433959] UDP-Lite hash table entries: 256 (order: 0, 4096 bytes)
[   42.434234] NET: Registered protocol family 1
[   42.434692] RPC: Registered udp transport module.
[   42.434722] RPC: Registered tcp transport module.
[   42.434753] RPC: Registered tcp NFSv4.1 backchannel transport module.
[   42.435058] Trying to unpack rootfs image as initramfs...
[   42.437805] rootfs image is not initramfs (no cpio magic); looks like an initrd
[   43.128540] Freeing initrd memory: 131072K
[   43.129364] omap-iommu omap-iommu.0: isp registered
[   43.131195] VFS: Disk quotas dquot_6.5.2
[   43.131317] Dquot-cache hash table entries: 1024 (order 0, 4096 bytes)
[   43.132415] squashfs: version 4.0 (2009/01/31) Phillip Lougher
[   43.133270] JFFS2 version 2.2. (NAND) (SUMMARY)   2001-2006 Red Hat, Inc.
[   43.134124] msgmni has been set to 874
[   43.138031] alg: No test for stdrng (krng)
[   43.138366] Block layer SCSI generic (bsg) driver version 0.4 loaded (major 254)
[   43.138427] io scheduler noop registered
[   43.138458] io scheduler deadline registered
[   43.138610] io scheduler cfq registered (default)
[   43.198638] OMAP DSS rev 2.0
[   43.198730] OMAP DISPC rev 3.0
[   43.198791] OMAP VENC rev 2
[   43.199066] OMAP DSI rev 1.0
[   43.535430] Serial: 8250/16550 driver, 4 ports, IRQ sharing enabled
[   43.554473] serial8250.0: ttyS0 at MMIO 0x4806a000 (irq = 72) is a ST16654
[   43.572845] serial8250.1: ttyS1 at MMIO 0x4806c000 (irq = 73) is a ST16654
[   43.591186] serial8250.2: ttyS2 at MMIO 0x49020000 (irq = 74) is a ST16654
[   44.191162] console [ttyS2] enabled
[   44.201507] brd: module loaded
[   44.207977] loop: module loaded
[   44.212249] omap2-nand driver initializing
[   44.216766] NAND device: Manufacturer ID: 0x20, Chip ID: 0xba (ST Micro NAND 256MiB 1,8V 16-bit)
[   44.226226] cmdlinepart partition parsing not available
[   44.231567] Creating 5 MTD partitions on "omap2-nand":
[   44.236785] 0x000000000000-0x000000080000 : "X-Loader"
[   44.243133] 0x000000080000-0x000000260000 : "U-Boot"
[   44.249633] 0x000000260000-0x000000280000 : "U-Boot Env"
[   44.255767] 0x000000280000-0x000000680000 : "Kernel"
[   44.263031] 0x000000680000-0x000010000000 : "File System"
[   44.363250] usbcore: registered new interface driver catc
[   44.368713] catc: v2.8:CATC EL1210A NetMate USB Ethernet driver
[   44.374847] usbcore: registered new interface driver kaweth
[   44.380493] pegasus: v0.6.14 (2006/09/27), Pegasus/Pegasus II USB Ethernet driver
[   44.388153] usbcore: registered new interface driver pegasus
[   44.393920] rtl8150: v0.6.2 (2004/08/27):rtl8150 based usb-ethernet driver
[   44.400939] usbcore: registered new interface driver rtl8150
[   44.406738] usbcore: registered new interface driver asix
[   44.412292] usbcore: registered new interface driver cdc_ether
[   44.418304] usbcore: registered new interface driver dm9601
[   44.424011] usbcore: registered new interface driver smsc95xx
[   44.429931] usbcore: registered new interface driver gl620a
[   44.435638] usbcore: registered new interface driver net1080
[   44.441467] usbcore: registered new interface driver plusb
[   44.447082] usbcore: registered new interface driver rndis_host
[   44.453155] usbcore: registered new interface driver cdc_subset
[   44.459228] usbcore: registered new interface driver zaurus
[   44.464965] usbcore: registered new interface driver MOSCHIP usb-ethernet driver
[   44.473083] ehci_hcd: USB 2.0 'Enhanced' Host Controller (EHCI) Driver
[   44.479980] ehci-omap ehci-omap.0: OMAP-EHCI Host Controller
[   44.485961] ehci-omap ehci-omap.0: new USB bus registered, assigned bus number 2
[   44.493621] ehci-omap ehci-omap.0: irq 77, io mem 0x48064800
[   44.510925] ehci-omap ehci-omap.0: USB 2.0 started, EHCI 1.00
[   44.516845] usb usb2: New USB device found, idVendor=1d6b, idProduct=0002
[   44.523742] usb usb2: New USB device strings: Mfr=3, Product=2, SerialNumber=1
[   44.531066] usb usb2: Product: OMAP-EHCI Host Controller
[   44.536437] usb usb2: Manufacturer: Linux 2.6.32 ehci_hcd
[   44.541900] usb usb2: SerialNumber: ehci-omap.0
[   44.547210] hub 2-0:1.0: USB hub found
[   44.551086] hub 2-0:1.0: 3 ports detected
[   44.581451] Initializing USB Mass Storage driver...
[   44.586547] usbcore: registered new interface driver usb-storage
[   44.592651] USB Mass Storage support registered.
[   44.597686] mice: PS/2 mouse device common for all mice
[   44.603332] input: gpio-keys as /devices/platform/gpio-keys/input/input0
[   44.611175] input: twl4030_pwrbutton as /devices/platform/i2c_omap.1/i2c-1/1-0049/twl4030_pwrbutton/input/input1
[   44.621948] i2c /dev entries driver
[   44.626068] Linux video capture interface: v2.00
[   44.631042] omap-iommu omap-iommu.0: isp: version 1.1
[   44.637451] vpfe_init
[   44.640350] OMAP Watchdog Timer Rev 0x31: initial timeout 60 sec
[   44.753662] mmci-omap-hs mmci-omap-hs.1: err -16 configuring card detect
[   44.760742] Registered led device: beagleboard::usr0
[   44.765899] Registered led device: beagleboard::usr1
[   44.772399] Registered led device: beagleboard::pmu_stat
[   44.779449] usbcore: registered new interface driver usbhid
[   44.785125] usbhid: USB HID core driver
[   44.789184] Advanced Linux Sound Architecture Driver Version 1.0.21.
[   44.796142] usbcore: registered new interface driver snd-usb-audio
[   44.878112] usb 2-2: new high speed USB device using ehci-omap and address 2
[   44.886138] No device for DAI omap-mcbsp-dai-0
[   44.890625] No device for DAI omap-mcbsp-dai-1
[   44.895172] No device for DAI omap-mcbsp-dai-2
[   44.899658] No device for DAI omap-mcbsp-dai-3
[   44.904174] No device for DAI omap-mcbsp-dai-4
[   44.908660] OMAP3 Beagle SoC init
[   44.912872] asoc: twl4030 <-> omap-mcbsp-dai-0 mapping ok
[   44.924407] ALSA device list:
[   44.927490]   #0: omap3beagle (twl4030)
[   44.931457] oprofile: using arm/armv7
[   44.935424] TCP cubic registered
[   44.938690] NET: Registered protocol family 17
[   44.943267] NET: Registered protocol family 15
[   44.947845] lib80211: common routines for IEEE802.11 drivers
[   44.953643] ThumbEE CPU extension supported.
[   44.957977] Power Management for TI OMAP3.
[   44.963348] Unable to set L3 frequency (400000000)
[   44.968292] Switched to new clocking rate (Crystal/Core/MPU): 26.0/332/1000 MHz
[   44.975708] IVA2 clocking rate: 800 MHz
[   45.143707] SmartReflex driver initialized
[   45.147979] omap3beaglelmb: Driver registration complete
[   45.158752] VFP support v0.3: implementor 41 architecture 3 part 30 variant c rev 3
[   45.167114] registered taskstats version 1
[   45.171997] fbcvt: 1024x768@60: CVT Name - .786M3-R
[   45.206634] usb 2-2: New USB device found, idVendor=0424, idProduct=9514
[   45.213409] usb 2-2: New USB device strings: Mfr=0, Product=0, SerialNumber=0
[   45.221954] hub 2-2:1.0: USB hub found
[   45.225860] hub 2-2:1.0: 5 ports detected
[   45.296905] Console: switching to colour frame buffer device 128x48
[   45.312866] regulator_init_complete: incomplete constraints, leaving VAUX3 on
[   45.320648] regulator_init_complete: incomplete constraints, leaving VDAC on
[   45.328277] omap_vout omap_vout: probed for an unknown device
[   45.335327] RAMDISK: gzip image found at block 0
[   45.342132] mmc0: new SD card at address bf53
[   45.355285] mmcblk0: mmc0:bf53 SU01G 968 MiB 
[   45.360076]  mmcblk0: p1 p2
[   45.520172] usb 2-2.1: new high speed USB device using ehci-omap and address 3
[   45.652770] usb 2-2.1: New USB device found, idVendor=0424, idProduct=ec00
[   45.667541] usb 2-2.1: New USB device strings: Mfr=0, Product=0, SerialNumber=0
[   45.686431] smsc95xx v1.0.4
[   45.777984] usb0: register 'smsc95xx' at usb-ehci-omap.0-2.1, smsc95xx USB 2.0 Ethernet, fe:11:e6:dc:ff:25
[   47.980834] VFS: Mounted root (ext2 filesystem) on device 1:0.
[   47.986999] devtmpfs: mounted
[   47.992645] Freeing init memory: 204K

INIT: version 2.86 booting

Please wait: booting...
Starting udev
[   50.414184] FAT: bogus number of reserved sectors
[   50.421447] VFS: Can't find a valid FAT filesystem on dev mmcblk0.
[   50.766143] FAT: bogus number of reserved sectors
[   50.773529] VFS: Can't find a valid FAT filesystem on dev mmcblk0p2.
Remounting root file system...
Caching udev devnodes
Populating dev cache
Configuring network interfaces... ifconfig: SIOCGIFFLAGS: No such device
udhcpc (v1.13.2) started
Sending discover...
[   53.210418] usb0: link up, 100Mbps, full-duplex, lpa 0x45E1
Sending discover...
Sending select for 192.168.1.65...
Lease of 192.168.1.65 obtained, lease time 86400
adding dns 192.168.1.1
done.
Setting up IP spoofing protection: rp_filter.
hwclock: can't open '/dev/misc/rtc': No such file or directory
Fri Aug 20 20:54:00 UTC 2010
hwclock: can't open '/dev/misc/rtc': No such file or directory
Configuring update-modules.
Configuring ti-dsplink-module.
Configuring ti-lpm-module.
Configuring util-linux-ng.
update-alternatives: Linking //bin/dmesg to dmesg.util-linux-ng
update-alternatives: Linking //bin/kill to kill.util-linux-ng
update-alternatives: Linking //bin/more to more.util-linux-ng
update-alternatives: Linking //sbin/mkswap to mkswap.util-linux-ng
update-alternatives: Linking //sbin/pivot_root to pivot_root.util-linux-ng
update-alternatives: Linking //sbin/sln to sln.util-linux-ng
update-alternatives: Linking //sbin/mkfs.minix to mkfs.minix.util-linux-ng
update-alternatives: Linking //sbin/fsck.minix to fsck.minix.util-linux-ng
update-alternatives: Linking //usr/bin/hexdump to hexdump.util-linux-ng
update-alternatives: Linking //usr/bin/last to last.sysvinit
update-alternatives: Linking //usr/bin/logger to logger.util-linux-ng
update-alternatives: Linking //usr/bin/mesg to mesg.sysvinit
update-alternatives: Linking //usr/bin/renice to renice.util-linux-ng
update-alternatives: Linking //usr/bin/wall to wall.sysvinit
update-alternatives: Linking //usr/bin/chfn to chfn.util-linux-ng
update-alternatives: Linking //usr/bin/newgrp to newgrp.util-linux-ng
update-alternatives: Linking //usr/bin/chsh to chsh.util-linux-ng
update-alternatives: Linking //bin/login to login.util-linux-ng
update-alternatives: Error: not linking //sbin/vipw to vipw.util-linux-ng since //sbin/vipw exists and is not a link
update-alternatives: Linking //sbin/vigr to vigr.util-linux-ng
update-alternatives: Linking //usr/bin/reset to reset.util-linux-ng
update-alternatives: Linking //usr/bin/setsid to setsid.util-linux-ng
update-alternatives: Linking //usr/bin/chrt to chrt.util-linux-ng
update-alternatives: Linking //sbin/hwclock to ../bin/busybox
update-alternatives: Linking //sbin/shutdown to shutdown.sysvinit
update-alternatives: Linking //sbin/reboot to reboot.sysvinit
update-alternatives: Linking //sbin/halt to halt.sysvinit

INIT: Entering runlevel: 5

Creating Dropbear SSH server RSA host key.
Will output 1024 bit rsa secret key to '/etc/dropbear/dropbear_rsa_host_key'
Generating key, this may take a while...
Public key portion is:
ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAAAgwDdkQ8vp+ZQKbKpK78oRNo/4o86o4UCxO1e0D6LHxwtRfsF4O+wsIgWPe11M3bjxeYFM9b0TrsijaHiqEExrJ3WmdyY8XE50+ggSycfiC5/FB9VKtxL44N4TQaAk6jVFCSSRusV1uClBOJSkI2HXbLc4BktoaqSCX3Si04A42HFrBc7 root@beagleboard
Fingerprint: md5 85:17:52:88:f5:d6:16:9f:6d:33:4d:c7:26:36:a8:bc
Starting Dropbear SSH server: dropbear.
Starting syslogd/klogd: done

.-------.                                           
|       |                  .-.                      
|   |   |-----.-----.-----.| |   .----..-----.-----.
|       |     | __  |  ---'| '--.|  .-'|     |     |
|   |   |  |  |     |---  ||  --'|  |  |  '  | | | |
'---'---'--'--'--.  |-----''----''--'  '-----'-'-'-'
                -'  |
                '---'

The Angstrom Distribution beagleboard ttyS2

Angstrom 2010.7-test-20100820 beagleboard ttyS2

beagleboard login: 
```

At this point, you can use the username 'root' to login.  There is no password:

```
beagleboard login: root
root@beagleboard:~#
```

## Checking your mounted filesystems ##

For the standard validation image, this is the output of the `mount` command immediately after booting:
```
root@beagleboard:~# mount
rootfs on / type rootfs (rw)
/dev/root on / type ext2 (rw,relatime)
devtmpfs on /dev type devtmpfs (rw,relatime,size=158312k,nr_inodes=39578,mode=75
5)
proc on /proc type proc (rw,relatime)
sysfs on /sys type sysfs (rw,relatime)
none on /dev type tmpfs (rw,relatime,mode=755)
/dev/mmcblk0p1 on /media/mmcblk0p1 type vfat (rw,sync,relatime,fmask=0022,dmask=
0022,codepage=cp437,iocharset=iso8859-1,shortname=mixed,errors=remount-ro)
devpts on /dev/pts type devpts (rw,relatime,gid=5,mode=620)
usbfs on /proc/bus/usb type usbfs (rw,relatime)
tmpfs on /var/volatile type tmpfs (rw,relatime)
tmpfs on /dev/shm type tmpfs (rw,relatime,mode=777)
tmpfs on /media/ram type tmpfs (rw,relatime)
root@beagleboard:~#
```

Note the mmcblk0p1 entry, representing the VFAT partition on the SD card, from which you can access any files you placed there when you initially populated that partition.

## Testing the USER button ##

To test the USER button:
```
root@beagleboard:~# cat `which testuserbtn`
#!/bin/sh
evtest /dev/input/event0
root@beagleboard:~# testuserbtn
Input driver version is 1.0.0
Input device ID: bus 0x19 vendor 0x1 product 0x1 version 0x100
Input device name: "gpio-keys"
Supported events:
  Event type 0 (Sync)
  Event type 1 (Key)
    Event code 276 (ExtraBtn)
Testing ... (interrupt to exit)
```

Now, press the USER button a few times until you get tired of pressing the USER button:
```
ATesting ... (interrupt to exit)
Event: time 1282337751.745392, type 1 (Key), code 276 (ExtraBtn), value 1
Event: time 1282337751.745392, -------------- Report Sync ------------
Event: time 1282337751.858033, type 1 (Key), code 276 (ExtraBtn), value 0
Event: time 1282337751.858033, -------------- Report Sync ------------
Event: time 1282337752.237519, type 1 (Key), code 276 (ExtraBtn), value 1
Event: time 1282337752.237550, -------------- Report Sync ------------
Event: time 1282337752.343567, type 1 (Key), code 276 (ExtraBtn), value 0
Event: time 1282337752.343597, -------------- Report Sync ------------
```

When done, press CTRL-C (^C) to breatk out of the `evtest` command:
```
Event: time 1282337752.343597, -------------- Report Sync ---------
root@beagleboard:~#
```

## Testing Audio In and Out ##

This test optionally performs loopback if you don't have your own audio source.  It begins by asking you to connect the Audio In to the Audio Out on your BeagleBoard.

```
root@beagleboard:~# cat `which testaudio`
#!/bin/sh
sox -n /tmp/sine.wav synth 1.0 sine 1000.0
echo "Connect audio out to audio in..."
sleep 3
arecord -t wav -c 2 -r 44100 -f S16_LE -v /tmp/sine-in.wav &
PID=$!
echo "Recording..."
sleep 1
echo "Playing..."
play /tmp/sine.wav
kill -15 $PID
sleep 1
echo "Connect the audio out to speakers..."
sleep 3
echo "Playing..."
play /tmp/sine-in.wav
sox /tmp/sine-in.wav -n stat
root@beagleboard:~#
root@beagleboard:~# testaudio
Connect audio out to audio in...
```

You have a choice here of doing what it says or simply leaving the Audio Out connected to powered speakers.  If you don't connect the Audio Out to the Audio In, you'll want to have your audio source active on Audio In at this time to provide it with something to record for your test.  If you leave Audio Out connected to powered speakers, you'll soon hear a sinewave tone:

```
Recording...
Recording WAVE '/tmp/sine-in.wav' : Signed 16 bit Little Endian, Rate 44100 Hz, Stereo
Plug PCM: Hardware PCM card 0 'omap3beagle' device 0 subdevice 0
Its setup is:
  stream       : CAPTURE
  access       : RW_INTERLEAVED
  format       : S16_LE
  subformat    : STD
  channels     : 2
  rate         : 44100
  exact rate   : 44100 (44100/1)
  msbits       : 16
  buffer_size  : 22016
  period_size  : 512
  period_time  : 11609
  tstamp_mode  : NONE
  period_step  : 1
  avail_min    : 512
  period_event : 0
  start_threshold  : 1
  stop_threshold   : 22016
  silence_threshold: 0
  silence_size : 0
  boundary     : 1442840576
  appl_ptr     : 0
  hw_ptr       : 0
Playing...

Input File     : '/tmp/sine.wav'
Sample Size    : 16-bit (2 bytes)
Sample Encoding: signed (2's complement)
Channels       : 2
Sample Rate    : 44100


Time: 00:00.09 [00:00.91] of 00:01.00 (  9.3%) Output Buffer:   4.10K
Time: 00:00.19 [00:00.81] of 00:01.00 ( 18.6%) Output Buffer:   8.19K
Time: 00:00.28 [00:00.72] of 00:01.00 ( 27.9%) Output Buffer:  12.29K
Time: 00:00.37 [00:00.63] of 00:01.00 ( 37.2%) Output Buffer:  16.38K
Time: 00:00.46 [00:00.54] of 00:01.00 ( 46.4%) Output Buffer:  20.48K
Time: 00:00.56 [00:00.44] of 00:01.00 ( 55.7%) Output Buffer:  24.58K
Time: 00:00.65 [00:00.35] of 00:01.00 ( 65.0%) Output Buffer:  28.67K
Time: 00:00.74 [00:00.26] of 00:01.00 ( 74.3%) Output Buffer:  32.77K
Time: 00:00.84 [00:00.16] of 00:01.00 ( 83.6%) Output Buffer:  36.86K
Time: 00:00.93 [00:00.07] of 00:01.00 ( 92.9%) Output Buffer:  40.96K
Time: 00:01.00 [00:00.00] of 00:01.00 ( 100.0%) Output Buffer:  44.10K

Done.
Aborted by signal Terminated...
Connect the audio out to speakers...
```

At this point, if you connected your Audio Out to the audio in, you'll want to **quickly** switch it back to the powered speakers.  Whatever was recorded on the Audio In for the last few seconds will now be played on Audio Out.  If you connected Audio In to Audio Out, you'll now hear the sinewave tone if you connect your speakers fast enough:

```
Playing...

Input File     : '/tmp/sine-in.wav'
Sample Size    : 16-bit (2 bytes)
Sample Encoding: signed (2's complement)
Channels       : 2
Sample Rate    : 44100


Time: 00:00.09 [00:01.93] of 00:02.02 (  4.6%) Output Buffer:   4.10K
Time: 00:00.19 [00:01.83] of 00:02.02 (  9.2%) Output Buffer:   8.19K
Time: 00:00.28 [00:01.74] of 00:02.02 ( 13.8%) Output Buffer:  12.29K
Time: 00:00.37 [00:01.65] of 00:02.02 ( 18.4%) Output Buffer:  16.38K
Time: 00:00.46 [00:01.56] of 00:02.02 ( 23.0%) Output Buffer:  20.48K
Time: 00:00.56 [00:01.46] of 00:02.02 ( 27.6%) Output Buffer:  24.58K
Time: 00:00.65 [00:01.37] of 00:02.02 ( 32.2%) Output Buffer:  28.67K
Time: 00:00.74 [00:01.28] of 00:02.02 ( 36.8%) Output Buffer:  32.77K
Time: 00:00.84 [00:01.18] of 00:02.02 ( 41.4%) Output Buffer:  36.86K
Time: 00:00.93 [00:01.09] of 00:02.02 ( 46.0%) Output Buffer:  40.96K
Time: 00:01.02 [00:01.00] of 00:02.02 ( 50.6%) Output Buffer:  45.06K
Time: 00:01.11 [00:00.91] of 00:02.02 ( 55.2%) Output Buffer:  49.15K
Time: 00:01.21 [00:00.81] of 00:02.02 ( 59.8%) Output Buffer:  53.25K
Time: 00:01.30 [00:00.72] of 00:02.02 ( 64.4%) Output Buffer:  57.34K
Time: 00:01.39 [00:00.63] of 00:02.02 ( 69.0%) Output Buffer:  61.44K
Time: 00:01.49 [00:00.53] of 00:02.02 ( 73.6%) Output Buffer:  65.54K
Time: 00:01.58 [00:00.44] of 00:02.02 ( 78.2%) Output Buffer:  69.63K
Time: 00:01.67 [00:00.35] of 00:02.02 ( 82.8%) Output Buffer:  73.73K
Time: 00:01.76 [00:00.26] of 00:02.02 ( 87.4%) Output Buffer:  77.82K
Time: 00:01.86 [00:00.16] of 00:02.02 ( 92.0%) Output Buffer:  81.92K
Time: 00:01.95 [00:00.07] of 00:02.02 ( 96.6%) Output Buffer:  86.02K
Time: 00:02.02 [00:00.00] of 00:02.02 ( 100.0%) Output Buffer:  89.09K

Done.
Samples read:            178176
Length (seconds):      2.020136
Scaled by:         2147483647.0
Maximum amplitude:     0.999969
Minimum amplitude:    -0.561859
Midline amplitude:     0.219055
Mean    norm:          0.320698
Mean    amplitude:     0.167296
RMS     amplitude:     0.428635
Maximum delta:         0.072296
Minimum delta:         0.000000
Mean    delta:         0.010727
RMS     delta:         0.022317
Rough   frequency:          365
Volume adjustment:        1.000
root@beagleboard:~# 
```

## Testing I2C reads of the EDID ##

Refer back to the section on reading the EDID from u-boot to understand the fields:

```
root@beagleboard:~# cat `which testedid`
#!/bin/sh
i2cdump -y 3 0x50

root@beagleboard:~# testedid
No size specified (using byte-data access)
     0  1  2  3  4  5  6  7  8  9  a  b  c  d  e  f    0123456789abcdef
00: 00 ff ff ff ff ff ff 00 1e 6d 4a 4b fe 95 00 00    ........?mJK??..
10: 04 11 01 03 ea 22 1b 78 ea 32 31 a3 57 4c 9d 25    ?????"?x?21?WL?%
20: 11 50 54 a5 6a 80 31 4f 45 4f 61 4f 81 80 01 01    ?PT?j?1OEOaO????
30: 01 01 01 01 01 01 30 2a 00 98 51 00 2a 40 30 70    ??????0*.?Q.*@0p
40: 13 00 52 0e 11 00 00 1e 00 00 00 fd 00 38 4b 1e    ?.R??..?...?.8K?
50: 47 0b 00 0a 20 20 20 20 20 20 00 00 00 fc 00 4c    G?.?      ...?.L
60: 31 39 33 33 54 52 0a 20 20 20 20 20 00 00 00 fc    1933TR?     ...?
70: 00 20 0a 20 20 20 20 20 20 20 20 20 20 20 00 9e    . ?           .?
80: ff ff ff ff ff ff ff ff ff ff ff ff ff ff ff ff    ................
90: ff ff ff ff ff ff ff ff ff ff ff ff ff ff ff ff    ................
a0: ff ff ff ff ff ff ff ff ff ff ff ff ff ff ff ff    ................
b0: ff ff ff ff ff ff ff ff ff ff ff ff ff ff ff ff    ................
c0: ff ff ff ff ff ff ff ff ff ff ff ff ff ff ff ff    ................
d0: ff ff ff ff ff ff ff ff ff ff ff ff ff ff ff ff    ................
e0: ff ff ff ff ff ff ff ff ff ff ff ff ff ff ff ff    ................
f0: ff ff ff ff ff ff ff ff ff ff ff ff ff ff ff ff    ................
root@beagleboard:~#
```

## Test LEDs ##

```
root@beagleboard:~# cat `which testled`
#!/bin/sh
led="/sys/class/leds/beagleboard::"

get_led_state () {
usr0_trigger=`cat ${led}usr0/trigger | sed -e "s/^.*\[\(.*\)\].*$/\1/"`
usr1_trigger=`cat ${led}usr1/trigger | sed -e "s/^.*\[\(.*\)\].*$/\1/"`
usr0_brightness=`cat ${led}usr0/brightness`
usr1_brightness=`cat ${led}usr1/brightness`
echo "USR0 LED state ($usr0_brightness) [$usr0_trigger]"
echo "USR1 LED state ($usr1_brightness) [$usr1_trigger]"
}

get_led_state
save0t="$usr0_trigger"
save0b="$usr0_brightness"
save1t="$usr1_trigger"
save1b="$usr1_brightness"

echo "Turning LEDs on for 3 seconds..."
echo "none" > ${led}usr0/trigger
echo "none" > ${led}usr1/trigger
echo "1" > ${led}usr0/brightness
echo "1" > ${led}usr1/brightness
get_led_state
sleep 3

echo "Turning USR0 LED off for 3 seconds..."
echo "0" > ${led}usr0/brightness
get_led_state
sleep 3

echo "Turning USR1 LED off for 3 seconds..."
echo "0" > ${led}usr1/brightness
get_led_state
sleep 3

echo "Restoring LEDs to their original state..."
echo "$save0b" > ${led}usr0/brightness
echo "$save1t" > ${led}usr1/brightness
echo "$save0t" > ${led}usr0/trigger
echo "$save1t" > ${led}usr1/trigger
get_led_state

root@beagleboard:~# testled
USR0 LED state (0) [heartbeat]
USR1 LED state (0) [mmc0]
Turning LEDs on for 3 seconds...
USR0 LED state (1) [none]
USR1 LED state (1) [none]
Turning USR0 LED off for 3 seconds...
USR0 LED state (0) [none]
USR1 LED state (1) [none]
Turning USR1 LED off for 3 seconds...
USR0 LED state (0) [none]
USR1 LED state (0) [none]
Restoring LEDs to their original state...
USR0 LED state (255) [heartbeat]
USR1 LED state (0) [mmc0]
root@beagleboard:~# 
```

## Testing the camera port ##

## Testing USB OTG as a peripheral ##

_THIS IS CURRENTLY **BROKEN** IN THE VALIDATION IMAGE._

For this test, we will configure the BeagleBoard as a USB Ethernet Gadget.  This means that the BeagleBoard will look like a USB Ethernet dongle to the host PC and will also act as if it has a USB-based Ethernet dongle attached and these two virtual Ethernet adapters will act like they are connected to each other over an Ethernet network.

Using the recommended 5V power supply to power your BeagleBoard as described in previous steps, start the USB Ethernet Gadget driver before connecting up your host PC:
```
root@beagleboard:~# lsmod
Module                  Size  Used by    Not tainted
mt9t112                 9246  0
root@beagleboard:~# modprobe g_ether host_addr=00:dc:c8:f7:75:05 dev_addr=00:dd:dc:eb:6d:f1
[ 1664.185699] usb1: MAC 00:dd:dc:eb:6d:f1
[ 1664.197906] usb1: HOST MAC 00:dc:c8:f7:75:05
[ 1664.212921] g_ether gadget: Ethernet Gadget, version: Memorial Day 2008
[ 1664.232727] g_ether gadget: g_ether ready
root@beagleboard:~# ifconfig usb1 10.1.123.2 netmask 255.255.255.0
root@beagleboard:~# ip addr show usb1
3: usb1: <BROADCAST,MULTICAST,UP> mtu 1500 qdisc pfifo_fast qlen 1000
    link/ether 00:dd:dc:eb:6d:f1 brd ff:ff:ff:ff:ff:ff
    inet 10.1.123.2/24 brd 10.1.123.255 scope global usb1
root@beagleboard:~#
```

Connect your BeagleBoard to your host PC using a standard-A to mini-B cable:

If you are using a Windows PC and have not done the setup required for Window RNDIS configuration then your host PC machine will prompt for USB new hardware found screen.  Go through the steps below (courtesy Steve K), or otherwise continue by skipping this step.
  1. Select the Linux.inf as the driver as downloaded previously, if you don't find it automatically.  You can also find a copy in the Linux kernel sources at Documentation/usb/linux.inf.
  1. On the Windows PC, bring up "Network Connections" and look for the Device Name: Linux USB Ethernet/RNDIS Gadget.
  1. Right-mouse click for Properties.
  1. Scroll down to Internet Protocol (TCP/IP), select it, and press the Properties button.
  1. Select an IP address close to the one selected for your BeagleBoard.  This example uses an IP address of 10.1.123.1 and a subnet mask of 255.255.255.0.

On the terminal emulator connected to the BeagleBoard use 'ping' to test the connection. Press the Ctrl-C buttons to terminate ping:
```
root@beagleboard:~# ping 10.1.123.1
PING 10.1.123.1 (10.1.123.1): 56 data bytes
```

Connect to the BeagleBoard from your PC, optionally opening up a port to forward web connections.  Providing real bridging is desired, but that isn't always easy to configure.  Using ssh port forwarding provides a simple way of getting web access to your BeagleBoard, if you can't have full access through bridging.  This example assumes you have ssh installed on your Windows host, so you'll need to find that from somewhere.

```
C:\>ssh -R 8000:myproxy.com:80 root@10.1.123.2
root@10.1.123.2's password:
root@beagleboard:~# export http_proxy=http://localhost:8000/
root@beagleboard:~# wget http://beagleboard.org/
Connecting to localhost:8000 (127.0.0.1:8000)
wget: can't open 'index.html': File exists
root@beagleboard:~# rm index.html
root@beagleboard:~# wget http://beagleboard.org/
Connecting to localhost:8000 (127.0.0.1:8000)
index.html           100% |**********************| 17457  --:--:-- ETA
root@beagleboard:~#
```

## To Test DVI and S-Video Interfaces by streaming a Video File ##



> [root@beagleboard mmc]# svideo
[root@beagleboard mmc]# mplayer /sample\_video.avi

NOTE: The Video sample has been downloaded from
https://garage.maemo.org/tracker/download.php/54/269/2380/258/bug.avi

Should display a cartoon video on DVI and S-Video, Audio will be on audio-out (speakers)
[root@beagleboard /]# mount -t vfat /dev/mmcblk0p1 /mnt
[root@beagleboard /]# cd /mnt
Make Sure your player is running and Audio Line in is connected to board.
[root@beagleboard mmc]# arecord -t wav -c 2 -r 44100 -f S16\_LE -v k
Following output is expected on Console
Recording WAVE 'k' : Signed 16 bit Little Endian, Rate 44100 Hz, Stereo
Plug PCM: Hardware PCM card 0 'TWL4030' device 0 subdevice 0
Its setup is:
> stream       : CAPTURE
> access       : RW\_INTERLEAVED
> format       : S16\_LE
> subformat    : STD
> channels     : 2
> rate         : 44100
> exact rate   : 44100 (44100/1)
> msbits       : 16
> buffer\_size  : 32768
> period\_size  : 2048
> period\_time  : 46439
> tick\_time    : 7812
> tstamp\_mode  : NONE
> period\_step  : 1
> sleep\_min    : 0
> avail\_min    : 2048
> xfer\_align   : 2048
> start\_threshold  : 1
> stop\_threshold   : 32768
> silence\_threshold: 0
> silence\_size : 0
> boundary     : 1073741824
When ever you think you want to stop just press CONTRL+C
[root@beagleboard mmc]# aplay -t wav -c 2 -r 44100 -f S16\_LE -v k
Audio should be heard on Speakers, Following output is expected on console
Playing WAVE 'k' : Signed 16 bit Little Endian, Rate 44100 Hz, Stereo
Plug PCM: Hardware PCM card 0 'TWL4030' device 0 subdevice 0
Its setup is:
> stream       : PLAYBACK
> access       : RW\_INTERLEAVED
> format       : S16\_LE
> subformat    : STD
> channels     : 2
> rate         : 44100
> exact rate   : 44100 (44100/1)
> msbits       : 16
> buffer\_size  : 32768
> period\_size  : 2048
> period\_time  : 46439
> tick\_time    : 7812
> tstamp\_mode  : NONE
> period\_step  : 1
> sleep\_min    : 0
> avail\_min    : 2048
> xfer\_align   : 2048
> start\_threshold  : 32768
> stop\_threshold   : 32768
> silence\_threshold: 0
> silence\_size : 0
> boundary     : 1073741824
This tests validates read/write to MMC/SD card as the audio data is being written and read from MMC.

> [root@beagleboard mmc]# cd /
> [root@beagleboard mmc]# umount /mnt
[root@beagleboard mmc]# evtest /dev/input/event2
Press a Key on USB KeyBoard,

Example if "a" is pressed the following output is seen on Console:

Event: time 1657.754638, type 1 (Key), code 30 (A), value 1
Event: time 1657.754638, -------------- Report Sync ------------
Event: time 1657.964599, type 1 (Key), code 30 (A), value 0
Event: time 1657.964599, -------------- Report Sync ------------
Press CONTROL+C to come out of this test
[root@beagleboard mmc]# evtest /dev/input/event4
Press the Mouse button and observe the screen,

Example if Left button is pressed and released the following lines should get displayed on console

Event: time 1871.724792, -------------- Report Sync ------------
Event: time 1873.804687, type 1 (Key), code 272 (LeftBtn), value 1
Event: time 1873.804687, -------------- Report Sync ------------
Event: time 1873.964660, type 1 (Key), code 272 (LeftBtn), value 0
Event: time 1873.964660, -------------- Report Sync ------------
Moving the Mouse also results in Console messages
Event: time 1959.120635, -------------- Report Sync ------------
Event: time 1959.130676, type 2 (Relative), code 0 (X), value -21
Event: time 1959.130676, -------------- Report Sync ------------
Event: time 1959.140625, type 2 (Relative), code 0 (X), value -16
Press CONTROL+C to come out of this test
  1. root@beagleboard:~# reboot

While kernel boots:
> - Connect a HUSB HUB with Mini A connector to USB OTG port on beagleboard
> - Connect a USB Keyboard, Mouse to USB HUB

  * ernel boots**[root@beagleboard mmc]# evtest /dev/input/event2
Press a Key on USB KeyBoard,**

Example if "a" is pressed the following output is seen on Console:

Event: time 1657.754638, type 1 (Key), code 30 (A), value 1
Event: time 1657.754638, -------------- Report Sync ------------
Event: time 1657.964599, type 1 (Key), code 30 (A), value 0
Event: time 1657.964599, -------------- Report Sync ------------
Press CONTROL+C to come out of this test
[root@beagleboard mmc]# evtest /dev/input/event4
Press the Mouse button and observe the screen,

Example if Left button is pressed and released the following lines should get displayed on console

Event: time 1871.724792, -------------- Report Sync ------------
Event: time 1873.804687, type 1 (Key), code 272 (LeftBtn), value 1
Event: time 1873.804687, -------------- Report Sync ------------
Event: time 1873.964660, type 1 (Key), code 272 (LeftBtn), value 0
Event: time 1873.964660, -------------- Report Sync ------------
Moving the Mouse also results in Console messages
Event: time 1959.120635, -------------- Report Sync ------------
Event: time 1959.130676, type 2 (Relative), code 0 (X), value -21
Event: time 1959.130676, -------------- Report Sync ------------
Event: time 1959.140625, type 2 (Relative), code 0 (X), value -16
Press CONTROL+C to come out of this test
[root@beagleboard mmc]# ifconfig eth0 

&lt;ipaddress&gt;


Example:
[root@beagleboard mmc]# ifconfig eth0 172.24.191.49 up


&lt;6&gt;

eth0: link up, 10Mbps, half-duplex, lpa 0x0020
eth0: link up, 10Mbps, half-duplex, lpa 0x0020

[root@beagleboard mmc]# ping 172.24.191.1
PING 172.24.191.1 (172.24.191.1): 56 data bytes
64 bytes from 172.24.191.1: seq=0 ttl=64 time=2.136 ms
64 bytes from 172.24.191.1: seq=1 ttl=64 time=1.068 ms
64 bytes from 172.24.191.1: seq=2 ttl=64 time=1.038 ms
> OMAP3 beagleboard.org # ibus 2 0x64
OMAP3 beagleboard.org # imd 0x50 0 100

Should get some thing similar header:

0000: 00 ff ff ff ff ff ff 00 10 ac 24 40 5a 39 41 41    ..........$@Z9AA
0010: 1f 11 01 03 80 22 1b 78 ee ae a5 a6 54 4c 99 26    .....".x....TL.&
0020: 14 50 54 a5 4b 00 71 4f 81 80 01 01 01 01 01 01    .PT.K.qO........
0030: 01 01 01 01 01 01 30 2a 00 98 51 00 2a 40 30 70    ......0**..Q.**@0p
0040: 13 00 52 0e 11 00 00 1e 00 00 00 ff 00 50 4d 30    ..R..........PM0
0050: 36 31 37 38 32 41 41 39 5a 0a 00 00 00 fc 00 44    61782AA9Z......D
0060: 45 4c 4c 20 31 37 30 38 46 50 0a 20 00 00 00 fd    ELL 1708FP. ....
0070: 00 38 4c 1e 51 0e 00 0a 20 20 20 20 20 20 00 36    .8L.Q...      .6

Note the words "DELL 1708FP" which is the ID of the monitor.

> OMAP3 beagleboard.org # nand unlock
OMAP3 beagleboard.org # nand erase

Will delete complete NAND

> NOTE: Copy x-load.bin.ift and u-boot.bin onto MMC as mentioned above
> > Boot the Board with MMC/SD card (should use MLO and u-boot.bin)
> > Power the board by pressing the user button
> > fatload mmc 0 80200000 x-load.bin.ift

> nand unlock
> nandecc hw
> nand erase 0 80000
> nand write 80200000 0 20000
> nand write 80200000 20000 20000
> nand write 80200000 40000 20000
> nand write 80200000 60000 20000
> fatload mmc 0 80200000 u-boot.bin
> nand unlock
> nandecc sw
> nand erase 80000 160000
> nand write 80200000 80000 160000
> arecord -t wav -c 2 -r 8000 -f S16\_LE -v /mnt/mmc/rec\_8000.dat
arecord -t wav -c 2 -r 11025 -f S16\_LE -v /mnt/mmc/rec\_11025.dat
arecord -t wav -c 2 -r 12000 -f S16\_LE -v /mnt/mmc/rec\_12000.dat
arecord -t wav -c 2 -r 16000 -f S16\_LE -v /mnt/mmc/rec\_16000.dat
arecord -t wav -c 2 -r 22050 -f S16\_LE -v /mnt/mmc/rec\_22050.dat
arecord -t wav -c 2 -r 24000 -f S16\_LE -v /mnt/mmc/rec\_24000.dat
arecord -t wav -c 2 -r 32000 -f S16\_LE -v /mnt/mmc/rec\_32000.dat
arecord -t wav -c 2 -r 44100 -f S16\_LE -v /mnt/mmc/rec\_44100.dat
arecord -t wav -c 2 -r 48000 -f S16\_LE -v /mnt/mmc/rec\_48000.dat
> aplay -t wav -c 2 -r 8000 -f S16\_LE -v /mnt/mmc/rec\_8000.dat
aplay -t wav -c 2 -r 11025 -f S16\_LE -v /mnt/mmc/rec\_11025.dat
aplay -t wav -c 2 -r 12000 -f S16\_LE -v /mnt/mmc/rec\_12000.dat
aplay -t wav -c 2 -r 16000 -f S16\_LE -v /mnt/mmc/rec\_16000.dat
aplay -t wav -c 2 -r 22050 -f S16\_LE -v /mnt/mmc/rec\_22050.dat

<so on>


setenv bootargs console=ttyS2,115200n8 ramdisk\_size=32768 root=/dev/ram0 rw rootfstype=ext2 initrd=0x81600000,32M omapfb.video\_mode=1280x720MR-48@60

mmcinit;fatload mmc 0 0x80300000 uImage.bin;fatload mmc 0 0x81600000 ramdisk.gz;bootm 0x80300000;

These might work as well,

setenv bootargs console=ttyS2,115200n8 ramdisk\_size=32768 root=/dev/ram0 rw rootfstype=ext2 initrd=0x81600000,32M omapfb.video\_mode=720x480MR-16@60

setenv bootargs console=ttyS2,115200n8 ramdisk\_size=32768 root=/dev/ram0 rw rootfstype=ext2 initrd=0x81600000,32M omapfb.video\_mode=720x576MR-12@60

setenv bootargs console=ttyS2,115200n8 ramdisk\_size=32768 root=/dev/ram0 rw rootfstype=ext2 initrd=0x81600000,32M omapfb.video\_mode=1280x720MR-110@60
C:\>ssh -R 8000:myproxy.com:80 root@10.1.123.2
root@10.1.123.2's password:
root@beagleboard:~# export http_proxy=http://localhost:8000/
root@beagleboard:~# wget http://beagleboard.org/
Connecting to localhost:8000 (127.0.0.1:8000)
wget: can't open 'index.html': File exists
root@beagleboard:~# rm index.html
root@beagleboard:~# wget http://beagleboard.org/
Connecting to localhost:8000 (127.0.0.1:8000)
index.html           100% |**********************| 17457  --:--:-- ETA
root@beagleboard:~#
}}}
   # *Mount the MMC/SD Card *
{{{
}}}
   # *To Test Audio-In Interface*
{{{
}}}
{{{
}}}
{{{
}}}
{{{
}}}
{{{
}}}
   # *To Test Audio-Out Interface, Playback the recorded Audio*
{{{
}}}
{{{
}}}
{{{
}}}
   # *umount the SD/MMC card*
{{{
}}}
   # *To test USB HOST Port*
     * Connect a Keyboard, Mouse to a USB HUB
     * Connect a HUB to USB Host connector on Beagle
     # *To Test USB HOST using a USB Keyboard*
{{{
}}}
{{{
}}}
{{{
}}}
     # *To Test USB HOST using a USB Mouse*
{{{
}}}
{{{
}}}
{{{
}}}
{{{
}}}
   # *To Test USB OTG as HOST*
     * Remove the existing Cable Cable on USB OTG port
     * Connect a mini A to USB A converter to USB OTG port of Beagle Board
     * Connect a HUB to USB A connector.
     * Connect USB peripherals (as needed for validation)like Keyboad, Mouse, Ethernet to USB HUB
     * Due to some limitation in kernel we are not able to switch to OTG host dynamically, for now reboot the board.
{{{
}}}
     # *To Test USB OTG as HOST using a USB Keyboard*
{{{
}}}
{{{
}}}
{{{
}}}
     # *To Test USB OTG as HOST using a USB Mouse*
{{{
}}}
{{{
}}}
{{{
}}}
{{{
}}}
     # *To Test USB OTG as HOST using a USB Ethernet Dongle*
{{{
}}}
{{{
}}}

== Other Tests ==

   * How to check EDID in u-boot
{{{
}}}

   * How to delete a NAND flash when it is already flashed from u-boot
{{{
}}}

   * How to flash x-loader and u-boot from u-boot
{{{
}}}
{{{
}}}

   * How to record and play at different sampling rates
{{{
}}}
{{{
}}}

   * How to get your DVID monitor working for different resolutions
{{{
}}}```