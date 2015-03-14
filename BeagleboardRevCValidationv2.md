**NOTE:**  This page is a variation of the original validation page [here](http://code.google.com/p/beagleboard/wiki/BeagleboardRevCValidation).  It is still a work in progress, and simply reproduces the last half of the original page, at least for now.

If you have suggestions for this page, drop me a note: `rpjday@crashcourse.ca`.

# What's this all about, then? #

This page discusses the process of "validating" your Revision C2/C3 BeagleBoard.  In essence, it describes the steps you might want to take after unwrapping a brand new board, holding it up (carefully, by its edges, of course) and thinking, "Um, OK, now what?"  This process is meant to be completely non-destructive in that it shows you how to test the various components of your board without changing any of its contents or settings.  Whether you subsequently want to reflash any of it to bring it up to date is entirely up to you.

While there is more than one way to power up and connect to your board, I prefer to use a 5V power supply, and connect to the standard serial port via a USB/serial adapter using `minicom`, but you can use whatever variation works for you.

And now, to work.

# The setup #

## Formatting the MMC/SD card ##

For these validation tests, you'll want a MMC/SD card with a single, empty VFAT/FAT32-formatted partition.  If you already know how to do that, you can skip the rest of this section.

To format your card under Windows, you can do the following:

  * Recommended HP USB Disk Storage Format Tool 2.18:
> > http://www.digitalsignagetechnology.com/forum/download/file.php?id=25&sid=bfbab0908f1a2f8434855c9f9b80e889
  * Insert the Card writer/reader into the Windows machine.
  * Insert MMC/SD card into the card reader/writer
  * Open the HP USB Disk Storage Format Tool.
  * Select “FAT32 as File System”.
  * Click on “Start”.
  * After formatting is done Click “OK”

If you're doing this under Linux, then just:

  * Install whatever your distro equivalent is to the `dosfstools` package -- the package that comes with the `mkfs.vfat` command.  (Chances are good it's already there.)
  * Insert the MMC/SD card and note what device it appears as.  If you have a built-in card reader, it will probably show up as `/dev/sdb` or something similar.  Let's assume it's `/dev/sdb` for the remainder of these instructions.
  * As root, run `fdisk /dev/sdb`, create a single, primary, card-spanning partition and set its partition type to "c" (which represents "W95 FAT32 (LBA)"), save that new partitioning and exit `fdisk`.  (For a detailed walkthrough of this process, go [here](http://elinux.org/BeagleBoardBeginners#SD_card_setup).)
  * Finally, from the command line, run `mkfs.vfat /dev/sdb1` to VFAT-format that new partition.

In either case, you should now have an MMC/SD card with a single, VFAT-format, mountable partition that you have to fill with files.  Onward ...

**NOTE**:  There is some discussion as to whether you need to make that new partition bootable.  I haven't needed to do that, others report that they did.  It might be related to whether you have one or more than one partition on the card.  In any event, keep that in mind.

## Populating the MMC/SD card ##

Because we want our validation to be non-destructive, we can keep the contents of our MMC/SD card to a minimum, but there's one critical point we need to address.  The U-Boot loader that ships with the C2 and C3 versions of the BeagleBoard is simply too old, and you **must** copy a much newer version to your SD card for proper validation.  To that end, once you format your card, copy to it the following (in exactly this order and with these destination names):

  1. [u-boot](http://www.angstrom-distribution.org/demo/beagleboard/u-boot.bin) as **u-boot.bin**
  1. [ramdisk image](http://beagleboard.googlecode.com/files/ramdisk_revc_v3.gz) as **ramdisk.gz**
  1. [Kernel (uImage)](http://beagleboard.googlecode.com/files/uImage_revc_v3.bin) as **uImage.bin**
  1. [normal.scr](http://beagleboard.googlecode.com/files/normal_revc_v3.scr) as **boot.scr**

Some notes about the above:

  * Some instructions tell you to first copy an `MLO` file to your SD card.  This would be the initial X-Loader image but, as far as I can tell, the X-Loader that ships with the C2 and C3 boards is perfectly fine and there is no need to override it.  You can copy it if you want, but it won't make any difference.  (Addendum:  Apparently, the older Rev B boards _did_ have a broken X-Loader so, yes, you should copy a good `MLO` file to your card.  But if you have a Rev C card, you should be fine.)

  * The U-Boot image above comes from the Angstrom distribution, and it is sufficiently new for what we want.

In short, for our validation, the above is all you should need.  Personally, I keep all of the relevant files in a single directory and use the following shell script to clear and repopulate my card whenever I need to:

```
#!/bin/sh

SD=$1

if [ $# = 0 ] ; then
        echo "No SD directory specified, exiting."
        exit
fi

echo "Clearing files from ${SD}."

rm -rf ${SD}/*

echo "Copying files to ${SD} ..."

cp u-boot-angstrom.bin ${SD}/u-boot.bin
cp ramdisk_revc_v3.gz ${SD}/ramdisk.gz
cp uImage_revc_v3.bin ${SD}/uImage.bin
cp normal_revc_v3.scr ${SD}/boot.scr

cp -a files ${SD}
```

You can probably tell that the single argument to pass to that script is the mount point of the SD card.

## Any other files you should copy to your MMC/SD card? ##

Technically, you don't need to populate your MMC/CD card with anything but what's listed above, but you're free to add any other files **after** copying the essential ones if you want access to them after booting.  Those extra files should have no effect on the boot process, but you'll be able to see them after booting once you mount the MMC/SD card.  So if you want to add a few extra audio or video files that you'd like to test, feel free.

For example, here's a .wav file [911.wav](http://www.wav-sounds.com/funny/911.wav) that I've verified will play with `aplay`.

## Board and peripheral setup ##

Depending on what you want to test and what peripherals you have at hand, do as much of the following as possible.

  1. Make sure Beagle power is in OFF state
  1. Connect UART cable to a UART port of Window/Linux/Mac machine
  1. Have terminal program, such as [TeraTerm](http://en.wikipedia.org/wiki/TeraTerm), HyperTerminal, or [Minicom](http://en.wikipedia.org/wiki/Minicom), running on the host machine.
  1. Configure the terminal program for (BAUD RATE - 115200, DATA - 8 bit, PARITY- none, STOP - 1bit, FLOW CONTROL - none)
  1. Insert the MMC/SD card (that is prepared as described above) into MMC/SD slot on Beagle Board.
  1. Connect a LCD Monitor to DVI/HDMI port on Beagle Board.
  1. Connect an externally powered speaker to audio out jack on Beagle Board.
  1. Connect a Line-in cable from PC or any player to Audio In jack on Beagle Board.
  1. Connect a TV (NTSC-M) to S-video port.
  1. Power ON LCD, TV and audio speakers.
  1. Connect one end of USB mini B to USB A cable to Beagle Board. **Do NOT** connect the USB A side to the host machine yet.
  1. If you have Windows PC as a host machine then copy the [Linux.inf](http://beagleboard.googlecode.com/files/linux.inf) RNDIS driver configuration file to the host machine. Generally Windows will have the driver.
  1. Also for Windows PCs, copy the [Gserial.inf](http://beagleboard.googlecode.com/files/gserial.inf) USB serial port driver configuration to the host machine.  Generally Windows will have the driver.

# Validation at the U-Boot level #

Regardless of what OS you plan on booting into, you can interrupt the boot process to stop in the U-Boot loader and take a look around to verify that everything looks sane, at least with respect to U-Boot.  If you're interested in what's happening at the U-Boot level, here are a few things you can check.

## version ##

First, there's the version of U-Boot you're loading (corresponding to the current version that's downloadable from the Angstrom distribution):

```
OMAP3 beagleboard.org # version                                                 
                                                                                
U-Boot 2009.08 (Oct 20 2009 - 13:09:07)                                         
OMAP3 beagleboard.org # 
```

## bdinfo ##

Next, you can examine the board information:

```
OMAP3 beagleboard.org # bdinfo                                                  
arch_number = 0x0000060A                                                        
env_t       = 0x00000000                                                        
boot_params = 0x80000100                                                        
DRAM bank   = 0x00000000                                                        
-> start    = 0x80000000                                                        
-> size     = 0x08000000                                                        
DRAM bank   = 0x00000001                                                        
-> start    = 0x88000000                                                        
-> size     = 0x08000000                                                        
                                                                                
baudrate    = 115200 bps                                                        
OMAP3 beagleboard.org # 
```

The RAM bank information should be self-explanatory, while the architecture number represents the official ARM designation of this particular board, as listed [here](http://www.arm.linux.org.uk/developer/machines/).  Converting hex '60A' to decimal gives 1546, which you can see is, in fact, the BeagleBoard in that list.  If that arch\_number value shows anything other than 0x60A, something has gone horribly wrong.

## coninfo ##

Next, print out the console information:

```
OMAP3 beagleboard.org # coninfo
List of available devices:
serial   80000003 SIO stdin stdout stderr 
OMAP3 beagleboard.org # 
```

## nand ##

List the NAND flash information with:

```
OMAP3 beagleboard.org # nand info

Device 0: nand0, sector size 128 KiB

```

Curiously, the older version of U-Boot was slightly more informative:

```
OMAP3 beagleboard.org # nand info                                               
                                                                                
Device 0: NAND 256MiB 1,8V 16-bit, sector size 128 KiB                          
OMAP3 beagleboard.org #
```

How odd.

## mtdparts ##

You can see the partition breakdown of flash with:

```
OMAP3 beagleboard.org # mtdparts 
mtdparts variable not set, see 'help mtdparts'
no partitions defined

defaults:
mtdids  : nand0=nand
mtdparts: mtdparts=nand:512k(x-loader),1920k(u-boot),128k(u-boot-env),4m(kernel),-(fs)
```

See the output of `help mtdparts` to see what else you can do here in terms of setting environment variables.

## i2c ##

The `i2c` command has a number of subcommands, but to simply see the I2C information, run:

```
OMAP3 beagleboard.org # i2c probe                                                 
Valid chip addresses: 48 49 4A 4B                                               
OMAP3 beagleboard.org # 
```

At the moment, I'm not sure what else you can do with `i2c` to display more I2C information, so I'm open to suggestions.  There's supposed to be a way to probe the EDID information of an attached display.  If you know what that is, let me know.

## mmc ##

To initialize your MMC/SD card, run:

```
OMAP3 beagleboard.org # mmc init
mmc1 is available
OMAP3 beagleboard.org # 
```

Once that's done, you can examine the contents of your FAT partition:

```
OMAP3 beagleboard.org # fatls mmc 1    (default / directory)
    20392   mlo 
   184316   u-boot.bin 
  7999649   ramdisk.gz 
  2578044   uimage.bin 
      603   boot.scr 
            files/

5 file(s), 1 dir(s)

OMAP3 beagleboard.org # fatls mmc 1 files
            ./
            ../
   186860   911.wav 

1 file(s), 2 dir(s)

OMAP3 beagleboard.org # 
```

## mtest ##

If you want to read/write test your RAM:

```
OMAP3 beagleboard.org # help mtest
mtest - simple RAM read/write test

Usage:
mtest [start [end [pattern [iterations]]]]
OMAP3 beagleboard.org # mtest 0x80000000 0x81000000 0xdeadbeef 1
Pattern DEADBEEF  Writing...            
... be patient ...
```

# Booting into Linux #

While I'm leaving the remainder of the original page below as is (for now) because of its detail, I'll summarize what you can do once you're done with messing around in U-Boot and now want to let your BeagleBoard boot to Linux.

These tests are based on the original (Arago-based) root filesystem that is used for validation, but a lot of it should still be relevant for other versions of Linux.

## Checking your mounted filesystems ##

For the standard validation, this is the output of the `mount` command immediately after booting:
```
root@beagleboard:/# mount
rootfs on / type rootfs (rw)
/dev/root on / type ext2 (rw,errors=continue)
proc on /proc type proc (rw)
sysfs on /sys type sysfs (rw)
none on /dev type tmpfs (rw,mode=755)
devpts on /dev/pts type devpts (rw,gid=5,mode=620)
usbfs on /proc/bus/usb type usbfs (rw)
tmpfs on /var/volatile type tmpfs (rw)
tmpfs on /dev/shm type tmpfs (rw,mode=777)
tmpfs on /media/ram type tmpfs (rw)
/dev/mmcblk0p1 on /media/mmcblk0p1 type vfat (rw,sync,fmask=0022,dmask=0022,codepage=cp437,iocharset=iso)
root@beagleboard:/# 
```
Note that last entry, representing the VFAT partition on the MMC/SD card, from which you can access any files you placed there when you initially populated that partition.

## Testing the USER button ##

Run
```
# evtest /dev/input/event1
```
and press the USER button until you get tired of pressing the USER button.  Then BREAK out of the `evtest` command.

## More Linux-level validation coming soon ... ##

It's not clear what else is worth discussing in terms of board validation.  If you've made it this far, you can assume you have a working board.  Going much further than this in terms of, say, checking audio and/or video starts to fall outside of scope of simple validation, and there are other web pages that already get into that sort of thing.

# Page revision has been done up to here, original material below still to be updated #

# REMAINDER OF ORIGINAL PAGE STARTS HERE #

# The boot process #

**Press the "User" switch/button on the Beagle Board.  While it’s still pressed, give power to (turn ON) the Beagle Board, by inserting the USB A connector side of the USB cable to HOST machine, then release the switch.**

This should bring up u-boot and the following Board Diagnostic tests will be performed:
  * The UART terminal should start displaying u-boot messages
  * A Beagle Board Logo will appear on DVI LCD screen.
  * Color bars should be displayed on TV over S-Video port.
  * The terminal screen should look like
```
40V

Texas Instruments X-Loader 1.4.2 (Feb 19 2009 - 12:01:24)
Reading boot sector
Loading u-boot.bin from mmc


U-Boot 2009.01-dirty (Feb 19 2009 - 12:23:21)

I2C:   ready
OMAP3530-GP rev 2, CPU-OPP2 L3-165MHz
OMAP3 Beagle board + LPDDR/NAND
DRAM:  256 MB
NAND:  256 MiB
Using default environment

MUSB: using high speed
In:    serial usbtty
Out:   serial usbtty
Err:   serial usbtty
Board revision C
Serial #7f6800030000000004013f780601a005
Hit any key to stop autoboot:  0
reading boot.scr
```
  * The DDR Size will be displayed in boot messages as
```
DRAM:  256 MB
```
  * The Board revision will be displayed in boot messages as
```
Board revision C
```
  * If `x-load.bin.ift` was on MMC, NAND will be flashed and additional lines will be displayed as
```
Running bootscript from mmc ...
## Executing script at 82000000
reading x-load.bin.ift

20392 bytes read
***** NAND will be Flashed with new x-loader and u-boot *****
***** Replacing x-load *****
device 0 whole chip
HW ECC selected

NAND erase: device 0 offset 0x0, size 0x80000
Erasing at 0x60000 -- 100% complete.
OK

NAND write: device 0 offset 0x0, size 0x20000
 131072 bytes written: OK

NAND write: device 0 offset 0x20000, size 0x20000
 131072 bytes written: OK

NAND write: device 0 offset 0x40000, size 0x20000
 131072 bytes written: OK

NAND write: device 0 offset 0x60000, size 0x20000
 131072 bytes written: OK
***** Replacing u-boot *****
reading u-boot.bin

727684 bytes read
device 0 whole chip
SW ECC selected

NAND erase: device 0 offset 0x80000, size 0x160000
Erasing at 0x1c0000 -- 100% complete.
OK

NAND write: device 0 offset 0x80000, size 0x160000
 1441792 bytes written: OK
***** Erasing environment settings *****
device 0 whole chip

NAND erase: device 0 offset 0x160000, size 0x20000
Erasing at 0x160000 -- 100% complete.
OK

```
  * If `uImage.bin` and `ramdisk.gz` are on MMC/SD card then these images will be read
```
reading uImage.bin

2578012 bytes read
***** Kernel: /dev/mmcblk0p1/uImage.bin *****
reading ramdisk.gz

33554432 bytes read
***** RootFS: /dev/mmcblk0p1/ramdisk.gz *****

```

Other tests will continue after Kernel booting, If `uImage.bin` and `ramdisk.gz` are on MMC card then kernel will boot as shown below ....

  * **Kernel boots**
```
## Booting kernel from Legacy Image at 80200000 ...
   Image Name:   Linux-2.6.28-omap1
   Image Type:   ARM Linux Kernel Image (uncompressed)
   Data Size:    2577980 Bytes =  2.5 MB
   Load Address: 80008000
   Entry Point:  80008000
   Verifying Checksum ... OK
   Loading Kernel Image ... OK
OK

Starting kernel ...

Uncompressing Linux.............................................................
................................................................................
........................ done, booting the kernel.
Linux version 2.6.28-omap1 (root@tiioss) (gcc version 4.2.1 (CodeSourcery Source
ry G++ Lite 2007q3-51)) #2 Thu Feb 19 12:45:34 IST 2009
CPU: ARMv7 Processor [411fc083] revision 3 (ARMv7), cr=10c5387f
CPU: VIPT nonaliasing data cache, VIPT nonaliasing instruction cache
Machine: OMAP3 Beagle Board
Memory policy: ECC disabled, Data cache writeback
OMAP3430 ES3.0
SRAM: Mapped pa 0x40200000 to va 0xd7000000 size: 0x100000
Reserving 15728640 bytes SDRAM for VRAM
Built 1 zonelists in Zone order, mobility grouping on.  Total pages: 65024
Kernel command line: console=ttyS2,115200n8 console=tty0 root=/dev/ram0 rw ramdi
sk_size=32768 initrd=0x81600000,32M
```
```
< followed by other messages till it reaches console ...>
```
```

.-------.
|       |                  .-.
|   |   |-----.-----.-----.| |   .----..-----.-----.
|       |     | __  |  ---'| '--.|  .-'|     |     |
|   |   |  |  |     |---  ||  --'|  |  |  '  | | | |
'---'---'--'--'--.  |-----''----''--'  '-----'-'-'-'
                -'  |
                '---'

The Angstrom Distribution beagleboard ttyS2

Angstrom 2008.1-test-20090127 beagleboard ttyS2

beagleboard login:

```
```
Type root and <hit the ENTER Key>
```



# Board validation #

  1. **To Test USB OTG as Peripheral (USB Ethernet Gadget with Windows HOST machine)**
    * If you have not done the setup required for Window RNDIS configuration then Host machine will prompt for USB new hardware found screen, go through the steps below (courtesy Steve K) to do the same , other wise continue by skipping this step
```
1. Select the Linux.inf as the driver, if you don't find it automatically, 
then you can copy one from Linux Kernel Source folder (2.6_kernel_revb-v2.tar.gz), 
from the path "2.6_kernel/Documentation/usb/linux.inf"
```
```
2. On the Windows PC, bring up Network Connections 
   and look for the Device Name: Linux USB Ethernet/RNDIS Gadget. 
   Right-mouse click for Properties. 
   Scroll down to Internet Protocol (TCP/IP), select it, 
   and press the Properties button.
```
```
3. Select an IP address close to the one selected for the Beagle Board. 
   This example uses an IP address of 192.168.1.5 and a Subnet mask of 255.255.255.0.
```
```
4. Configure a static IP address for Beagle Board with the ifconfig command. 
   The example below configures an IP address of 192.168.1.1 with a subnet mask of 255.255.255.0.

   [root@beagleboard mmc]# ifconfig usb0 192.168.1.1 netmask 255.255.255.0
```
```
5. On the terminal emulator connected to the Beagle Board use ping to test the connection. Press the Ctrl-C buttons to terminate ping.
```
```
[root@beagleboard mmc]# ping 192.168.1.5
PING 192.168.1.5 (192.168.1.5): 56 data bytes
64 bytes from 192.168.1.5: seq=0 ttl=128 time=0.885 ms
64 bytes from 192.168.1.5: seq=1 ttl=128 time=0.977 ms
64 bytes from 192.168.1.5: seq=2 ttl=128 time=0.977 ms
```
```
do a 
[root@beagleboard mmc]#ifconfig usb0 down
```
  1. **To Test DVI and S-Video Interfaces by streaming a Video File**
```

 [root@beagleboard mmc]# svideo
[root@beagleboard mmc]# mplayer /sample_video.avi

NOTE: The Video sample has been downloaded from 
https://garage.maemo.org/tracker/download.php/54/269/2380/258/bug.avi

Should display a cartoon video on DVI and S-Video, Audio will be on audio-out (speakers)
```
  1. **To Test Audio-In Interface**
```
Make Sure your player is running and Audio Line in is connected to board.
```
```
[root@beagleboard mmc]# arecord -t wav -c 2 -r 44100 -f S16_LE -v k
```
```
Following output is expected on Console
```
```
Recording WAVE 'k' : Signed 16 bit Little Endian, Rate 44100 Hz, Stereo
Plug PCM: Hardware PCM card 0 'TWL4030' device 0 subdevice 0
Its setup is:
  stream       : CAPTURE
  access       : RW_INTERLEAVED
  format       : S16_LE
  subformat    : STD
  channels     : 2
  rate         : 44100
  exact rate   : 44100 (44100/1)
  msbits       : 16
  buffer_size  : 32768
  period_size  : 2048
  period_time  : 46439
  tick_time    : 7812
  tstamp_mode  : NONE
  period_step  : 1
  sleep_min    : 0
  avail_min    : 2048
  xfer_align   : 2048
  start_threshold  : 1
  stop_threshold   : 32768
  silence_threshold: 0
  silence_size : 0
  boundary     : 1073741824
```
```
When ever you think you want to stop just press CONTRL+C
```
  1. **To Test Audio-Out Interface, Playback the recorded Audio**
```
[root@beagleboard mmc]# aplay -t wav -c 2 -r 44100 -f S16_LE -v k
```
```
Audio should be heard on Speakers, Following output is expected on console
```
```
Playing WAVE 'k' : Signed 16 bit Little Endian, Rate 44100 Hz, Stereo
Plug PCM: Hardware PCM card 0 'TWL4030' device 0 subdevice 0
Its setup is:
  stream       : PLAYBACK
  access       : RW_INTERLEAVED
  format       : S16_LE
  subformat    : STD
  channels     : 2
  rate         : 44100
  exact rate   : 44100 (44100/1)
  msbits       : 16
  buffer_size  : 32768
  period_size  : 2048
  period_time  : 46439
  tick_time    : 7812
  tstamp_mode  : NONE
  period_step  : 1
  sleep_min    : 0
  avail_min    : 2048
  xfer_align   : 2048
  start_threshold  : 32768
  stop_threshold   : 32768
  silence_threshold: 0
  silence_size : 0
  boundary     : 1073741824
```
  1. **To test USB HOST Port**
    * Connect a Keyboard, Mouse to a USB HUB
    * Connect a HUB to USB Host connector on Beagle
    1. **To Test USB HOST using a USB Keyboard**
```
[root@beagleboard mmc]# evtest /dev/input/event2
```
```
Press a Key on USB KeyBoard, 

Example if "a" is pressed the following output is seen on Console:

Event: time 1657.754638, type 1 (Key), code 30 (A), value 1
Event: time 1657.754638, -------------- Report Sync ------------
Event: time 1657.964599, type 1 (Key), code 30 (A), value 0
Event: time 1657.964599, -------------- Report Sync ------------
```
```
Press CONTROL+C to come out of this test
```
    1. **To Test USB HOST using a USB Mouse**
```
[root@beagleboard mmc]# evtest /dev/input/event4
```
```
Press the Mouse button and observe the screen,

Example if Left button is pressed and released the following lines should get displayed on console

Event: time 1871.724792, -------------- Report Sync ------------
Event: time 1873.804687, type 1 (Key), code 272 (LeftBtn), value 1
Event: time 1873.804687, -------------- Report Sync ------------
Event: time 1873.964660, type 1 (Key), code 272 (LeftBtn), value 0
Event: time 1873.964660, -------------- Report Sync ------------
```
```
Moving the Mouse also results in Console messages
Event: time 1959.120635, -------------- Report Sync ------------
Event: time 1959.130676, type 2 (Relative), code 0 (X), value -21
Event: time 1959.130676, -------------- Report Sync ------------
Event: time 1959.140625, type 2 (Relative), code 0 (X), value -16
```
```
Press CONTROL+C to come out of this test
```
  1. **To Test USB OTG as HOST**
    * Remove the existing Cable Cable on USB OTG port
    * Connect a mini A to USB A converter to USB OTG port of Beagle Board
    * Connect a HUB to USB A connector.
    * Connect USB peripherals (as needed for validation)like Keyboad, Mouse, Ethernet to USB HUB
    * Due to some limitation in kernel we are not able to switch to OTG host dynamically, for now reboot the board.
```
     # root@beagleboard:~# reboot

While kernel boots:
   - Connect a HUSB HUB with Mini A connector to USB OTG port on beagleboard
   - Connect a USB Keyboard, Mouse to USB HUB

     *Kernel boots*
```
    1. **To Test USB OTG as HOST using a USB Keyboard**
```
[root@beagleboard mmc]# evtest /dev/input/event2
```
```
Press a Key on USB KeyBoard, 

Example if "a" is pressed the following output is seen on Console:

Event: time 1657.754638, type 1 (Key), code 30 (A), value 1
Event: time 1657.754638, -------------- Report Sync ------------
Event: time 1657.964599, type 1 (Key), code 30 (A), value 0
Event: time 1657.964599, -------------- Report Sync ------------
```
```
Press CONTROL+C to come out of this test
```
    1. **To Test USB OTG as HOST using a USB Mouse**
```
[root@beagleboard mmc]# evtest /dev/input/event4
```
```
Press the Mouse button and observe the screen,

Example if Left button is pressed and released the following lines should get displayed on console

Event: time 1871.724792, -------------- Report Sync ------------
Event: time 1873.804687, type 1 (Key), code 272 (LeftBtn), value 1
Event: time 1873.804687, -------------- Report Sync ------------
Event: time 1873.964660, type 1 (Key), code 272 (LeftBtn), value 0
Event: time 1873.964660, -------------- Report Sync ------------
```
```
Moving the Mouse also results in Console messages
Event: time 1959.120635, -------------- Report Sync ------------
Event: time 1959.130676, type 2 (Relative), code 0 (X), value -21
Event: time 1959.130676, -------------- Report Sync ------------
Event: time 1959.140625, type 2 (Relative), code 0 (X), value -16
```
```
Press CONTROL+C to come out of this test
```
    1. **To Test USB OTG as HOST using a USB Ethernet Dongle**
```
[root@beagleboard mmc]# ifconfig eth0 <ipaddress>
```
```
Example:
[root@beagleboard mmc]# ifconfig eth0 172.24.191.49 up
<6>eth0: link up, 10Mbps, half-duplex, lpa 0x0020
eth0: link up, 10Mbps, half-duplex, lpa 0x0020

[root@beagleboard mmc]# ping 172.24.191.1
PING 172.24.191.1 (172.24.191.1): 56 data bytes
64 bytes from 172.24.191.1: seq=0 ttl=64 time=2.136 ms
64 bytes from 172.24.191.1: seq=1 ttl=64 time=1.068 ms
64 bytes from 172.24.191.1: seq=2 ttl=64 time=1.038 ms
```

## Other Tests ##

  * How to check EDID in u-boot
```
 OMAP3 beagleboard.org # ibus 2 0x64 
OMAP3 beagleboard.org # imd 0x50 0 100

Should get some thing similar header:

0000: 00 ff ff ff ff ff ff 00 10 ac 24 40 5a 39 41 41    ..........$@Z9AA
0010: 1f 11 01 03 80 22 1b 78 ee ae a5 a6 54 4c 99 26    .....".x....TL.&
0020: 14 50 54 a5 4b 00 71 4f 81 80 01 01 01 01 01 01    .PT.K.qO........
0030: 01 01 01 01 01 01 30 2a 00 98 51 00 2a 40 30 70    ......0*..Q.*@0p
0040: 13 00 52 0e 11 00 00 1e 00 00 00 ff 00 50 4d 30    ..R..........PM0
0050: 36 31 37 38 32 41 41 39 5a 0a 00 00 00 fc 00 44    61782AA9Z......D
0060: 45 4c 4c 20 31 37 30 38 46 50 0a 20 00 00 00 fd    ELL 1708FP. ....
0070: 00 38 4c 1e 51 0e 00 0a 20 20 20 20 20 20 00 36    .8L.Q...      .6

Note the words "DELL 1708FP" which is the ID of the monitor.

```

  * How to delete a NAND flash when it is already flashed from u-boot
```
 OMAP3 beagleboard.org # nand unlock
OMAP3 beagleboard.org # nand erase

Will delete complete NAND

```

  * How to flash x-loader and u-boot from u-boot
```
 NOTE: Copy x-load.bin.ift and u-boot.bin onto MMC as mentioned above
      Boot the Board with MMC/SD card (should use MLO and u-boot.bin)
      Power the board by pressing the user button
```
```
  fatload mmc 0 80200000 x-load.bin.ift
 nand unlock
 nandecc hw
 nand erase 0 80000
 nand write 80200000 0 20000
 nand write 80200000 20000 20000
 nand write 80200000 40000 20000
 nand write 80200000 60000 20000
 fatload mmc 0 80200000 u-boot.bin
 nand unlock
 nandecc sw
 nand erase 80000 160000
 nand write 80200000 80000 160000
```

  * How to record and play at different sampling rates
```
 arecord -t wav -c 2 -r 8000 -f S16_LE -v /mnt/mmc/rec_8000.dat 
arecord -t wav -c 2 -r 11025 -f S16_LE -v /mnt/mmc/rec_11025.dat 
arecord -t wav -c 2 -r 12000 -f S16_LE -v /mnt/mmc/rec_12000.dat 
arecord -t wav -c 2 -r 16000 -f S16_LE -v /mnt/mmc/rec_16000.dat 
arecord -t wav -c 2 -r 22050 -f S16_LE -v /mnt/mmc/rec_22050.dat 
arecord -t wav -c 2 -r 24000 -f S16_LE -v /mnt/mmc/rec_24000.dat 
arecord -t wav -c 2 -r 32000 -f S16_LE -v /mnt/mmc/rec_32000.dat 
arecord -t wav -c 2 -r 44100 -f S16_LE -v /mnt/mmc/rec_44100.dat 
arecord -t wav -c 2 -r 48000 -f S16_LE -v /mnt/mmc/rec_48000.dat
```
```
 aplay -t wav -c 2 -r 8000 -f S16_LE -v /mnt/mmc/rec_8000.dat 
aplay -t wav -c 2 -r 11025 -f S16_LE -v /mnt/mmc/rec_11025.dat 
aplay -t wav -c 2 -r 12000 -f S16_LE -v /mnt/mmc/rec_12000.dat 
aplay -t wav -c 2 -r 16000 -f S16_LE -v /mnt/mmc/rec_16000.dat 
aplay -t wav -c 2 -r 22050 -f S16_LE -v /mnt/mmc/rec_22050.dat

<so on>

```

  * How to get your DVID monitor working for different resolutions
```

setenv bootargs console=ttyS2,115200n8 ramdisk_size=32768 root=/dev/ram0 rw rootfstype=ext2 initrd=0x81600000,32M omapfb.video_mode=1280x720MR-48@60

mmcinit;fatload mmc 0 0x80300000 uImage.bin;fatload mmc 0 0x81600000 ramdisk.gz;bootm 0x80300000;

These might work as well,

setenv bootargs console=ttyS2,115200n8 ramdisk_size=32768 root=/dev/ram0 rw rootfstype=ext2 initrd=0x81600000,32M omapfb.video_mode=720x480MR-16@60

setenv bootargs console=ttyS2,115200n8 ramdisk_size=32768 root=/dev/ram0 rw rootfstype=ext2 initrd=0x81600000,32M omapfb.video_mode=720x576MR-12@60

setenv bootargs console=ttyS2,115200n8 ramdisk_size=32768 root=/dev/ram0 rw rootfstype=ext2 initrd=0x81600000,32M omapfb.video_mode=1280x720MR-110@60

```