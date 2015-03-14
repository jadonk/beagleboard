# Beagle Board Hardware Setup #

## Beagle Board Setup for NAND Booting ##
  * Make sure Beagle is powered OFF
  * Connect RS232 port on Beagle to UART port of Windows/Linux Machine using RS232 Null Modem Cable
  * Have Terminal program (TeraTerm, HyperTerminal or Minicom) running on the host machine.
  * Configure the Terminal program for (BAUD RATE - 115200, DATA - 8 bit, PARITY- none, STOP - 1bit, FLOW CONTROL - none)
  * Connect a LCD Monitor to DVI/HDMI port on Beagle Board _(Optional)_.
  * Connect an externally powered speaker to audio out jack on Beagle Board _(Optional)_.
  * Connect a TV (NTSC-M) to S-video port _(Optional)_.
  * Power ON LCD, TV and audio speakers _(Optiona)_.
  * Power ON the Beagle board with an External +5v Power Supply

## Beagle Board Setup for MMC booting ##

**MMC /SD Card Formatting Procedure**

  * Gather all required Tools.
    * MMC/SD Card
    * MMC/SD Card writer
    * MMC/SD Card Formatting/Partitioning Tool (To create a bootable partition on MMC/SD Card)
    * Recommended HP USB Disk Storage Format Tool 2.0.6:
> > > http://selfdestruct.net/misc/usbboot/SP27213.exe

  * Format the MMC/SD Card for FAT32 FS
    * Insert the Card writer/reader into the Windows machine.
    * Insert MMC/SD card into the card reader/writer
    * Open the HP USB Disk Storage Format Tool.
    * Select “FAT32 as File System”.
    * Click on “Start”.
    * After formatting is done Click “OK”

  * To Format MMC/SD card on a Linux machine, please refer to LinuxBootDiskFormat.

  * Follow Compilation steps to build the required binaries or use the Pre-built images as described at http://code.google.com/p/beagleboard/wiki/BeagleSourceCode

  * Copy MLO, u-boot.bin onto MMC/SD Card

**Beagle Boad Setup for Booting over MMC /SD Card**

  * Make sure Beagle is powered OFF
  * Connect RS232 port on Beagle to UART port of Windows/Linux Machine using RS232 Null Modem Cable
  * Have Terminal program (TeraTerm, HyperTerminal or Minicom) running on the host machine.
  * Configure the Terminal program for (BAUD RATE - 115200, DATA - 8 bit, PARITY- none, STOP - 1bit, FLOW CONTROL - none)
  * Insert the MMC/SD card that has kernel, u-boot, MLO images into MMC/SD slot on Beagle Board.
  * Connect a LCD Monitor to DVI/HDMI port on Beagle Board_(Optional)_.
  * Connect an externally powered speaker to audio out jack on Beagle Board_(Optional)_.
  * Connect a TV (NTSC-M) to S-video port_(Optional)_.
  * Power ON LCD, TV and audio speakers_(Optional)_.
  * Power ON the Beagle board with an External +5v Power Supply