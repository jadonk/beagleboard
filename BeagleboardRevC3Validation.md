# Validating Beagle Board (Rev C4 and Rev Bx) with Tests / Diagnostics #

**SIDE NOTE**:  There is a slightly newer version of this page [here](http://code.google.com/p/beagleboard/wiki/BeagleboardRevCValidationv2) that is still a work in progress but you might find it helpful.  Feel free to check it out.

## Setup ##

**Prepare MMC/SD card for Validation**

  * Format the MMC/SD Card for FAT32 FS
  * Recommended HP USB Disk Storage Format Tool 2.18:
**http://www.pcworld.com/downloads/file/fid,64963-page,1/description.html**

**http://files.extremeoverclocking.com/file.php?f=197**

**http://hp-usb-disk-storage-format-tool.software.informer.com/**

  * Insert the Card writer/reader into the Windows machine.
  * Insert MMC/SD card into the card reader/writer
  * Open the HP USB Disk Storage Format Tool.
  * Select “FAT32 as File System”....NOTE: In some cases FAT32 does not work correctly on some cards. If this happens, select FAT during this step.
  * Click on “Start”.
  * After formatting is done Click “OK”

**Copy the following files on to MMC**in the following order**:**

  1. [MLO](http://beagleboard.googlecode.com/files/MLO_revc_v3) as **MLO**
  1. [u-boot](http://beagleboard.googlecode.com/files/u-boot_revc_v3.bin) as **u-boot.bin**
  1. [u-boot for flash](http://beagleboard.googlecode.com/files/u-boot-f_revc_v3.bin) as **u-boot-f.bin**
  1. [ramdisk image](http://beagleboard.googlecode.com/files/ramdisk_revc_v3.gz) as **ramdisk.gz**
  1. [Kernel (uImage)](http://beagleboard.googlecode.com/files/uImage_revc_v3.bin) as **uImage.bin**
  1. [reset.scr](http://beagleboard.googlecode.com/files/reset_revc_v3.scr) as **boot.scr**
  1. [x-loader image](http://beagleboard.googlecode.com/files/x-load_revc_v3.bin.ift) as **x-load.bin.ift**
  1. [Regular script file](http://beagleboard.googlecode.com/files/normal_revc_v3.scr) as **normal.scr**





NOTE:
If you don't want to flash the bootloader on NAND then copy [Regular script file](http://beagleboard.googlecode.com/files/normal_revc_v3.scr) as **boot.scr**



NOTE: If the board is a Rev C4, use the files located here http://www.angstrom-distribution.org/demo/beagleboard/  instead of the ones listed above.




**Sources for board testing can be found at**

  * x-loader    : http://gitorious.org/x-load-omap3/mainline commit: 037a8ed45e9ecfffacfaab0b7a713fdde56d155a
  * u-boot      : http://gitorious.org/beagleboard/mainline branch: omap3-dev-usb commit: 3f6a11d8a37f11bef4611345b3d3fe38cc086a17
  * Diagnostic kernel      : http://gitorious.org/projects/beagleboard-diagnostic-kernel branch: 2.6.28-oe-[r8](https://code.google.com/p/beagleboard/source/detail?r=8)
  * file system : http://arago-project.org/wiki/index.php/Main_Page (Arago Project)




**Board Setup for Validating**

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

## Beagle validation ##

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
  * If x-load.bin.ift was on MMC, NAND will be flashed and additional lines will be displayed as
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
  * If uImage.bin and ramdisk.gz are on MMC/SD card then these images will be read
```
reading uImage.bin

2578012 bytes read
***** Kernel: /dev/mmcblk0p1/uImage.bin *****
reading ramdisk.gz

33554432 bytes read
***** RootFS: /dev/mmcblk0p1/ramdisk.gz *****

```

Other tests will continue after Kernel booting, If uImage.bin and ramdisk.gz are on MMC card then kernel will boot as shown below ....

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
Type root and <hit a ENTER Key>
```

Continue Board Validation

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
  1. **Mount the MMC/SD Card**
```
[root@beagleboard /]# mount -t vfat /dev/mmcblk0p1 /mnt
[root@beagleboard /]# cd /mnt
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
  1. **umount the SD/MMC card**
```
This tests validates read/write to MMC/SD card as the audio data is being written and read from MMC.

         [root@beagleboard mmc]# cd /
         [root@beagleboard mmc]# umount /mnt
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