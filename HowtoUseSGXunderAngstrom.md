# Introduction #
This may not be perfect, but will give you a head start should you decide to try the OpenGL/ES drivers for the SGX in the OMAP 35x.  Please remember that this software is alpha quality.  So it does work, but it has many problems.

# Known Problems #

Each of the below problems is software related and either has a work-around or will be addressed in some upcoming release.

  * Requires 16 bits-per-pixel framebuffer operation.  To ensure this, add 'omapfb.video\_mode=1024x768MR-16@60' to the bootargs command in your u-boot environment.  Note that the -16 is what's important.  The default is -24 which will not work.  Other resolutions will work.

  * Seems to show a vertical discontinuity (tearing) down the centerline when operating at 1024x768 resolution.

  * Seems to run the SGX block at half its normal speeed (55 MHz versus 110 MHz).  There is a workaround for this problem in the procedure below.

# Prerequisites #

  * This cookbook assumes that you have already installed Bitbake and Open Embedded.  And that you have an up-to-date copy of the openembedded metadata installed on a Linux machine.  Refer to: http://elinux.org/BeagleBoardAndOpenEmbeddedGit for instructions on installing bitbake and the metadata.  You will need to do the entire procedure.

  * You will need to have an SD card with the console image (or other Angstrom image) built and installed on it. Note that the SGX drivers here do not play that nicely with X11.  So a console image may be the best bet.

  * This procedure assumes that you are permitted to obtain the OMAP 35x Graphics SDK version 3.00.00.06 from Texas Instruments, and that you are willing and able to accept the terms of the click-wrap license agreement that accompanies this SDK.  Access to the SDK is controlled by Texas Instruments.  And this procedure does not include any rights or implication regarding rights to access this SDK.

  * This procedure assumes that your Beagleboard has a web connection via wired or wireless ethernet etc.  Usually this is done with a USB-to-Ethernet dongle plugged into a powered USB hub, which is, in turn, plugged into the Beagleboard.  See BeagleBoardShoppingList for some hints on ethernet interfaces.  The ethernet interface is needed simply for the opkg install commands listed below in Step IV.  If need be, there may be a way to do the opkg installs without network access.

# Procedure for Building the SGX User-mode Libraries for OpenGL/ES #

## Step I:  Obtain the Graphics SDK from TI ##

  * Obtain a http://my.ti.com account by visiting the site and registering appropriately.

  * Send an email to 'gamingonomap@list.ti.com' requesting extranet access to the OMAP35x\_Graphics\_SDK\_setuplinux\_3\_00\_00\_06.bin release through your my.ti.com account, and give them your my.ti.com username (email address).

  * Go to https://www-a.ti.com/downloads/sds_support/targetcontent/dvsdk/oslinux_dvsdk/v3_00_3530/index.html and log in with your my.ti.com credentials.

  * Download the file named:  OMAP35x\_Graphics\_SDK\_setuplinux\_3\_00\_00\_06.bin.  (I'll call it the .bin file for short).  It's about 270 MB.  So it may take a while.

  * Do a 'chmod +x' on the .bin file.  (If it's not executable the Bitbake will fail)

## Step II:  Build the user-mode libraries for OpenGL/es 2.0 and 1.1 ##

  * Copy the .bin file from Step I into your $OE\_HOME/openembedded/packages/powervr-drivers/ folder.

  * Do 'source $OE\_HOME/beagleboard/beagleboard/profile.sh' to establish the bitbake environment variables.

  * Do 'bitbake libgles-omap3-3.00.00.06'. This will fire off a build of the ipk files.  It should start a build with 500 or more steps.

  * Answer the dialog boxes with yes, or default answers.

  * When the bitbake completes, you will should have four .ipk files in:  $OE\_HOME/tmp/deploy/glibc/ipk/armv7a/ named:

```
     libgles-omap3_3.00.00.06-r5.1_armv7a.ipk
     libgles-omap3-dbg_3.00.00.06-r5.1_armv7a.ipk
     libgles-omap3-dev_3.00.00.06-r5.1_armv7a.ipk
     libgles-omap3-tests_3.00.00.06-r5.1_armv7a.ipk
```

  * Copy the four .ipk files onto your Angstrom SD Card in the /home/root folder of the Angstrom Linux Partition on the SD card.

  * Print out the file:  $OE\_HOME/tmp/work/armv7a-angstrom-linux-gnueabi/libgles-omap3-3.00.00.06-[r5](https://code.google.com/p/beagleboard/source/detail?r=5)/OMAP35x\_Graphics\_SDK\_3\_00\_00\_06/OMAP35x\_Graphics\_SDK\_GettingStartedGuide.pdf.  This file is full of information.  But remember:  This document is written for the Texas Instruments OMAP 35x EVM running the Texas Instruments Linux distribution.  So not everything in here applies directly to the Angstrom environment running on Beagleboard.

## Step III:  Configuring your Beagleboard for 16-bit Framebuffer Operation ##

By default, the Beagleboard boots with 24 bit-per-pixel frame buffer operation.  The SGX drivers do not support this mode.

  * Connect your Beagleboard's serial port to a terminal emulator (minicom, teraterm, etc).  Set the emulator software to 115200 baud, no parity, 8-bits, 1 stop-bit, no flow control.

  * Turn on your Beagleboard.  When it starts its countdown, hit any key to get to the u-boot prompt.

  * At the u-boot prompt, do:  'nand erase 260000 20000'.  This will erase the settings in the NAND and restore them to their defaults.

  * Do:  setenv mmcargs 'setenv bootargs console=ttyS2,115200n8 root=/dev/mmcblk0p2 rw rootfstype=ext3 rootwait omapfb.video\_mode=1024x768MR-16@60'

  * Do:  saveenv

  * Do:  'printenv' to see that the changes are there properly.

  * Now cycle power and the system should boot up with the monitor in 1024x768 60Hz resolution, with 16bpp.

## Step IV:  Install the Libraries in Your Angstrom System ##

  * Now put the SD Card in the Beagleboard, boot up Angstrom, and log in as root.

  * Make sure that your internet connection is working properly by doing a ping google.com or something like that.  Sometimes getting the DHCP to work requires cycling the power and trying again.  These opkg install and update commands use the internet to access the Angstrom package feeds and to download the appropriate packages as necessary.

  * Do 'opkg update'.  This may take a few minutes and it will update your local list of packages available from the Angstrom feeds.

  * Do 'opkg install /home/root/libgles-omap3\_3.00.00.06-[r5](https://code.google.com/p/beagleboard/source/detail?r=5).1\_armv7a.ipk'.  This will install the OpenGL/ES libraries onto your system.

  * Do 'opkg install /home/root/libgles-omap3-tests\_3.00.00.06-[r5](https://code.google.com/p/beagleboard/source/detail?r=5).1\_armv7a.ipk'.  This will install the SGX Test programs.

  * Do 'opkg install devmem2'.  This will install a little utility that will let you run the SGX at its normal speed of 110 MHz.

## Step V:  Running the SGX Software ##

  * Reboot your Beagleboard.

  * Do: 'dmesg | grep PVR'.  And you should see 'PVRSRV\_PIXEL\_FORMAT\_RGB565'.  If you get RGB888, then you are in 24-bpp mode and the software will not work.  Refer to Section III.

  * Do: 'devmem2 0x48004b40 w 0'.  This should change the register from an original value of 2 to a new value of 0.  The effect of this is that it will double the speed of the SGX accelerator.  But this change will need to be done each time you reboot.

  * Now you can run the test programs as described in section 3.4.1 of the OMAP35x Graphics SDK Getting Started Guide that was printed out in Section 2.  Note that not all the tests seem to run.  Again, note that this is not a product release but an early alpha stage release.