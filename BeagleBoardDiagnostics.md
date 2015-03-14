The current latest links to the diagnostics/validation page is at [BeagleboardRevCValidation](BeagleboardRevCValidation.md).

# Validating Beagle Board with Tests / Diagnostics #

## Setup ##

**Prepare MMC/SD card for Validation**

  * Format the MMC/SD Card for FAT32 FS
  * Recommended HP USB Disk Storage Format Tool 2.0.6:
> > http://selfdestruct.net/misc/usbboot/SP27213.exe
  * Insert the Card writer/reader into the Windows machine.
  * Insert MMC/SD card into the card reader/writer
  * Open the HP USB Disk Storage Format Tool.
  * Select “FAT32 as File System”.
  * Click on “Start”.
  * After formatting is done Click “OK”

**Copy the following files on to MMC**

  1. [x-load.bin.ift for NAND](http://beagleboard.googlecode.com/files/x-load.bin.ift_for_NAND) as **x-load.bin.ift**
  1. [MLO](http://beagleboard.googlecode.com/files/MLO_revb) as **MLO**
  1. [u-boot to flash onto NAND](http://beagleboard.googlecode.com/files/flash-uboot.bin) as **flash-uboot.bin**
  1. [u-boot.bin for MMC boot](http://beagleboard.googlecode.com/files/u-boot.bin_autoflash) as **u-boot.bin**
  1. [ramdisk Image](http://beagleboard.googlecode.com/files/rd-ext2-8M.bin) as **rd-ext2.bin**
  1. [Kernel (uImage)](http://beagleboard.googlecode.com/files/uImage_OTG) as **uImage**
  1. [Sample Video File](http://www.beagleboard.org/uploads/HARRY.YUV) as **HARRY.YUV**

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
  1. Connect one end of USB mini B to USB A cable to Beagle board. **Do NOT** connect the USB A side to Host machine yet.
  1. If you have Windows PC as a host machine then copy the [Linux.inf](http://beagleboard.googlecode.com/files/linux.inf) RNDIS driver configuration file to Host machine. Generally windows will have one.

## Beagle validation ##

**Press the "User" switch/button on the Beagle Board.  While it’s still pressed, give power to (turn ON) the Beagle Board, by inserting the USB A connector side of the USB cable to HOST machine, then release the switch.**

This should bring up u-boot and the following Board Diagnostic tests will be performed:
  * The UART terminal should start displaying u-boot messages
  * A Beagle Board Logo will appear on DVI LCD screen.
  * Color bars should be displayed on TV over S-Video port.
  * A tone will be played on Audio out / speakers.
  * If the boot command was not configured/saved then NAND flash on board will be flashed automatically,
Generally when board is in production bootcmd is empty.

  * The terminal screen should look like
```
40T

Texas Instruments X-Loader 1.41
Starting on with MMC
Reading boot sector

717372 Bytes Read from MMC
Starting OS Bootloader from MMC...


U-Boot 1.3.3 (Jul 10 2008 - 16:30:47)

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
```

  * If NAND was flashed additional lines will be displayed as
```
reading x-load.bin.ift

9808 bytes read
device 0 whole chip
nand_unlock: start: 00000000, length: 268435456!
NAND flash successfully unlocked

NAND erase: device 0 offset 0x0, size 0x80000
Erasing at 0x60000 -- 100% complete.
OK

NAND write: device 0 offset 0x0, size 0x80000

Writing data at 0x7f800 -- 100% complete.
 524288 bytes written: OK
reading flash-uboot.bin

717116 bytes read
device 0 whole chip
nand_unlock: start: 00000000, length: 268435456!
NAND flash successfully unlocked

NAND erase: device 0 offset 0x80000, size 0x160000
Erasing at 0x1c0000 -- 100% complete.
OK

NAND write: device 0 offset 0x80000, size 0x160000

Writing data at 0x1df800 -- 100% complete.
 1441792 bytes written: OK
OMAP3 beagleboard.org #
```

Other tests will continue after Kernel booting, follow the below procedure to continue...

  * Set the boot arguments for kernel
```
OMAP3 beagleboard.org # setenv bootargs console=ttyS2,115200n8 ramdisk_size=8192 root=/dev/ram0 rw rootfstype=ext2 initrd=0x81600000,8M nohz=0ff
```

  * Set boot command as
```
OMAP3 beagleboard.org # setenv bootcmd 'mmcinit;fatload mmc 0 0x80300000 uImage;fatload mmc 0 0x81600000 rd-ext2.bin;bootm 0x80300000';
```

  * Execute the boot command
```
OMAP3 beagleboard.org # run bootcmd
```
```
Following status will be displayed
reading uImage

1856680 bytes read
reading rd-ext2.bin

3394477 bytes read
```
  * **Kernel boots**
```
## Booting kernel from Legacy Image at 80300000 ...
   Image Name:   Linux-2.6.22.18-omap3
   Image Type:   ARM Linux Kernel Image (uncompressed)
   Data Size:    1856616 Bytes =  1.8 MB
   Load Address: 80008000
   Entry Point:  80008000
   Verifying Checksum ... OK
   Loading Kernel Image ... OK
OK

Starting kernel ...

Uncompressing Linux.............................................................
............................................................. done, booting the
kernel.
<5>Linux version 2.6.22.18-omap3 (root@fedoraserver) (gcc version 4.2.1 (CodeSou
rcery Sourcery G++ Lite 2007q3-51)) #1 Thu Jul 24 15:29:36 IST 2008
CPU: ARMv7 Processor [411fc082] revision 2 (ARMv7), cr=00c5387f
Machine: OMAP3 Beagle board
```
```
< followed by other messages till it reaches console ...>
```
```
System initialization complete.

Please press Enter to activate this console.
```
```
<hit a ENTER Key>
```

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
Right-mouse click for Properties. Scroll down to Internet Protocol (TCP/IP), select it, and press the Properties button.
```
```
3. Select an IP address close to the one selected for the Beagle Board. This example uses an IP address of 192.168.1.5 and a Subnet mask of 255.255.255.0.
```
```
4. Configure a static IP address for Beagle Board with the ifconfig command. The example below configures an IP address of 192.168.1.1 with a subnet mask of 255.255.255.0.
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
  1. **Mount the MMC/SD Card**
```
[root@beagleboard /]# mount -t vfat /dev/mmcblk0p1 /mnt/mmc/
[root@beagleboard /]# cd /mnt/mmc/
```
  1. **To Test DVI Interface by streaming a Video**
```
[root@beagleboard mmc]# stream_video
```
```
Should display a 320x240 video of Stephane Edberg playing Tennis on DVI screen
```
  1. **To Test S-Video Interface by streaming a Video**
```
[root@beagleboard mmc]# echo 'tv' > /sys/class/display_control/omap_disp_control
/video1
```
```
[root@beagleboard mmc]# stream_video
```
```
Should display a 320x240 video of Stephane Edberg playing Tennis on TV
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
  1. **Reboot the board to test USB HOST and to double check if NAND was flashed**
    1. umount the SD/MMC card
```
         [root@beagleboard mmc]# cd /
         [root@beagleboard mmc]# umount /mnt/mmc
```
    1. Remove the SD/MMC card from the slot
    1. Remove the existing Cable
    1. Connect a mini A to USB A converter to USB OTG port of Beagle Board
    1. Connect a HUB to USB A connector.
    1. Connect USB peripherals (as needed for validation)like Keyboad, Mouse, Ethernet to USB HUB
    1. Power On the Beagle board using Power Adapter and **Do NOT** press any switch
    1. This should boot u-boot from NAND
```
       Texas Instruments X-Loader 1.41
       Starting OS Bootloader...


       U-Boot 1.3.3 (Jul 10 2008 - 16:33:09)

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
```
```
       NOTE : Make Sure there is no such line "Starting on with MMC" in boot messages
```
    1. Re-Insert the MMC Card
    1. Again Boot the Kernel by following below steps
      * Set the boot arguments for kernel
```
OMAP3 beagleboard.org # setenv bootargs console=ttyS2,115200n8 ramdisk_size=8192 root=/dev/ram0 rw rootfstype=ext2 initrd=0x81600000,8M nohz=0ff
```
      * Set boot command as
```
OMAP3 beagleboard.org # setenv bootcmd 'mmcinit;fatload mmc 0 0x80300000 uImage;fatload mmc 0 0x81600000 rd-ext2.bin;bootm 0x80300000';
```
      * Execute the boot command
```
OMAP3 beagleboard.org # run bootcmd
```
```
Following status will be displayed
reading uImage

1856680 bytes read
reading rd-ext2.bin

3394477 bytes read
```
      * **Kernel boots**
  1. **To Test USB OTG as HOST using a USB Keyboard**
```
[root@beagleboard mmc]# evtest /dev/input/event1
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
[root@beagleboard mmc]# evtest /dev/input/event0
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
NOTE: Copy x-load.bin.ift and flash-uboot.bin onto MMC as mentioned above
      Boot the Board with MMC/SD card (should use MLO and u-boot.bin)
```
```
OMAP3 beagleboard.org # mmcinit;
OMAP3 beagleboard.org # fatload mmc 0 0x80200000 x-load.bin.ift;
OMAP3 beagleboard.org # nand unlock;
OMAP3 beagleboard.org # nand ecc hw;
OMAP3 beagleboard.org # nand erase 0 80000;
OMAP3 beagleboard.org # nand write.i 0x80200000 0 80000;
OMAP3 beagleboard.org # fatload mmc 0 0x80200000 flash-uboot.bin; nand unlock;
OMAP3 beagleboard.org # nand ecc sw;nand erase 80000 160000; 
OMAP3 beagleboard.org # nand write.i 0x80200000 80000 160000;\0"
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