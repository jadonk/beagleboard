# My Beagle Board Out of the box experience #

**What's in the box?**

Box should have a 3"x3" OMAP3530 Board.

**Is their any factory default software flashed onto this board?**

Yes, x-loader and u-boot should have been flashed on to the board.

**How can I execute factory defaults from Beagle Board?**

## Setup ##
  1. Make sure Beagle power is in OFF state
  1. Connect UART cable to a UART port of Window/Linux/Mac machine
  1. Have terminal program, such as [TeraTerm](http://en.wikipedia.org/wiki/TeraTerm), HyperTerminal, or [Minicom](http://en.wikipedia.org/wiki/Minicom), running on the host machine.
  1. Configure the terminal program for (BAUD RATE - 115200, DATA - 8 bit, PARITY- none, STOP - 1bit, FLOW CONTROL - none)
  1. Connect a LCD Monitor to DVI/HDMI port on Beagle Board.
  1. Connect an externally powered speaker to audio out jack on Beagle Board.
  1. Connect a TV (NTSC-M) to S-video port.
  1. Power ON LCD, TV and audio speakers.

NOTE: Connecting all peripherals (mentioned above) is not mandatory, start using the one you have.

## Powering Beagle Board ##
  1. Power ON the Beagle Board by one of the following options
    * Option A: External Power Adapter can be used to power the board.
    * Option B: USB mini B to USB A cable can be used to power the board. Just connect the USB A side to Host machine and USB mini A side of cable to Beagle board.

## Experience ##

This should bring up u-boot and the following Board Diagnostic tests will be performed:
  * The UART terminal should start displaying u-boot messages
  * A Beagle Board Logo will appear on DVI LCD screen.
  * Color bars should be displayed on TV over S-Video port.
  * A tone will be played on Audio out / speakers.
  * LEDs USR0 and USR1 will glow
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
OMAP3 beagleboard.org #
```

**What next?
  * Can try to compile Linux Kernel Image directly from [OMAP GIT](http://www.muru.com/linux/omap)
  * Can try some [sample images and sources](http://code.google.com/p/beagleboard/wiki/BeagleSourceCode)
  * Learn  [how to compile sources](http://code.google.com/p/beagleboard/wiki/BeagleSoftCompile) and generate images for Beagle
  * Try some quick [Demos](http://code.google.com/p/beagleboard/wiki/Demos)**

