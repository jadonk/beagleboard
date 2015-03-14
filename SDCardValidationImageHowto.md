# Introduction #

This page documents how you can create an SD Card image such as the ones at [Circuit Co's wiki page](http://www.circuitco.com/support/index.php?title=Main_Page).

# Steps #

## 1. Download / Prepare file system Image ##

### Narcissus image builder ###
Windows users can use [Narcissus](http://narcissus.angstrom-distribution.org), the online image builder, can build a an image for you].

Visit the above URL and,
  1. Select Machine as "beaglboard" and a suitable name.
  1. Select the option complexity as "Advanced"
  1. In the "Select the release you want to base your rootfs image on" dropdown, select "2011.03"
  1. Select the type of image "tar.gz"
  1. Head to the bottom of the page and under "Additional packages selection", select the checkbox for "Beagleboard validation GNOME image" and click on "Build me!".

### Bitbake ###
Or, Linux users can build the above image using [this recipe](http://cgit.openembedded.org/cgit.cgi/openembedded/tree/recipes/images/beagleboard-validation-gnome-image.bb?h=2011.03-maintenance).
For details on how to build an image with bitbake, [visit OpenEmbedded](http://www.openembedded.org). Remember to select tar.gz as the output package format in your build/local.conf file.

## 2. Prepare SD Card ##
  1. Insert a 4GB SD Card into your memory card reader.
  1. [Follow these instructions](http://elinux.org/BeagleBoardBeginners\#SD_card_setup) to create an SD Card of size 3GB. Note that the cylinder size you enter should be 360 for this. A bigger partition can create problems with writing the image from windows.
  1. Unpack the contents of the tar.gz Image into the ext2 (FS) partition of your card.
  1. From the ext2 (FS) parition, Copy /boot/uImage and /boot/MLO into the FAT32 partition of your card.
  1. Next, In a separate temp directory on your local machine, download and [these scripts](https://gitorious.org/beagleboard-validation/validation-scripts/archive-tarball/master) and unpack them.
  1. In this directory,
    1. Copy uEnv.txt and user.txt from beagleboard-validation-validation-scripts/flashing/ into the FAT32 (boot) partition of your card.

**The rest of the steps in this section are valid only if your BeagleBoard has NAND, and you want your SD Card image to flash to NAND and boot from there directly**

  1. Copy the tar.gz image you downloaded from Narcissus or from bitbake into /boot of the EXT2 (FS) partition.
  1. Edit /etc/init.d/flash-fs.sh to make sure TAR\_GZ\_IMG points to the tar.gz in /boot that you copied in the last step.

## 3. Boot your SD Card and make sure it works ##

**For those would like to flash to NAND**

To flash to NAND Hold the userbutton when you insert the SD Card and bootup. You will have to keep holding the userbutton until you see a "Writing to flash" message.

If all went well, you should be able to see in your serial console, that it is writing to NAND. Don't remove the SD Card until it tells you that it is safe to do so, then remove the card and reset. This time it will boot from NAND.

**For all**
Once you boot for the first time, wait for the configuration process to finish, it will take a while.

## 4. Making NAND flashing from SD Card image faster ##

Once and only once the above steps work for you, you can make make your SD Card image flashing process even faster.

Once, you followed step 3, insert the SD Card back into the board, and run the following command in serial console:

```
   mkdir -p /media/mmcfs
   mount /dev/mmcblk0p2 /media/mmcfs
   cat /dev/mtd4 | gzip > /media/mmcfs/mtd4.gz
```

Next power off and insert the SD Card into your local machine.
cd into the temporary directory where you unpacked the scripts.
Then,
```
cp flashing/flash-fs-fast.sh <your-ext2-fs-mount>/etc/rc5.d/S99flashfs 
chmod +x <your-ext2-fs-mount>/etc/rc5.d/S99flashfs
```

Unmount and remove card. Insert into board. Now follow step 3, it should write much faster to NAND, this time.

## 5. Writing the SD Card to an Image ##
Once you're done with step 4, poweroff the board, and eject the SD Card. Now you're ready to write it to an Image.

**Windows Users:**

Follow the instructions at [Circuit Co Wiki](http://circuitco.com/support/index.php?title=Main_Page)

[Software](https://launchpad.net/win32-image-writer) to use for reading or writing a card

**Linux Users:**

Writing to Image:
```
cat /dev/mmcblk0p2 | gzip | dd of=image-destination.gz
```
Writing to Card:
```
zcat image-destination.gz | dd of=/dev/mmcblk0p2 bs=10M
```

## 6. Troubleshooting ##
**Out of space errors when writing to NAND**

Make sure that the image you create in step 1 is big enough to fit on the NAND of your board (which depends on your board).