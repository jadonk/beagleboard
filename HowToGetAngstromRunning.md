# Introduction #

This is not the only way to do this.  But it is the way that I have found to work.  :-)

Plus, this procedure shows you how to boot up the Angstrom Demo image that has already been created and is on the web for download.

If you want to build your own Angstrom Images, then consult the [elinux.org How-To Page on Open Embedded Building](http://elinux.org/BeagleBoardAndOpenEmbeddedGit).


# Details #

## Section I.  Assembling the Pieces ##

When you buy a Beagleboard, it comes with one item -- the board itself.  This is not nearly enough to actually use a Beagleboard.  This section shows you how to get your Beagleboard accessories and to get set up to begin.

  * Review BeagleBoardShoppingList.  It has a list of many things that will be very helpful to you -- if you don't have them already.

  * Much of this procedure should be done from a Linux PC.  It's ok to use VMware machine running on a Windows PC.  See BeagleBoardShoppingList for a list of places that you can get the software needed if you're operating from a Windows PC.

  * So if you have a Windows PC, then:
    * Download VMware player
    * Download the 20GB Ubuntu image from the shopping list above.
    * Install the VMware Player.
    * Unpack the Ubuntu image into a folder.  (this may require the 7z unpacker -- also listed in the shopping list).
    * Start VMware, and Open the Ubuntu image that was unpacked.
    * Log in as jars/jars.

  * Follow the procedure at HowToConnectUpBeagleBoard to get your BeagleBoard properly connected to your PC.

  * Be sure you can get to the point of seeing the OMAP3 beagleboard# prompt (as below).

```
Texas Instruments X-Loader 1.41                                              
Starting OS Bootloader...                                                    
                                        
U-Boot 1.3.3 (Jul 10 2008 - 16:33:09)   
                                        
OMAP3530-GP rev 2, CPU-OPP2 L3-165MHz   
OMAP3 Beagle Board + LPDDR/NAND         
DRAM:  128 MB                           
NAND:  256 MiB
In:    serial
Out:   serial
Err:   serial
Audio Tone on Speakers  ... complete
OMAP3 beagleboard.org # 
```

## Section II.  Setting Up the Angstrom SD Card ##

  * Go to http://www.angstrom-distribution.org/demo/beagleboard/ and download the latest Angstrom-Beagleboard-Demo-Image....bz2 file, and all the other files located there into a folder on your Linux machine.

  * Go to LinuxBootDiskFormat and follow that procedure for making a dual-partition SD card.
    * Note that on your machine, it may be /dev/sdb -- not /dev/sdc -- for example.
    * I suggest giving partition 1 the label Beagle\_Boot, and partition 2 the label Angstrom\_Demo

  * Unplug, and replug in your SD card, and it should now automount the two partitions that you created.

  * IMPORTANT TO DO THIS BEFORE ANY OTHER FILES ARE COPIED:  Copy the MLO file that you downloaded to the Beagle\_Boot partition.  The MLO file must be the first file copied to the partition after re-formatting.

  * Copy the rest of the files downloaded to the Beagle\_Boot partition.

  * Now cd to the partition 2 -- e.g. cd /media/Angstrom\_Demo

  * Untar the filesystem.  e.g. tar -xvjf /media/Beagle\_Boot/Angstrom......bz2

  * Now you have a bootable SD card with the Angstrom demo fully loaded.

  * Unmount the SD card.
    * cd ~
    * sync
    * unmount /media/Beagle\_Boot
    * unmount /media/Angstrom\_Demo

## Section III.  Putting New X-Loader into Beagleboard NAND Flash Memory ##

The Rev B5 Beagleboards ship with an X-Loader that is not new enough to properly run Angstrom because it does not automatically use the uImage from the SD card (when present).  So this section upgrades the X-Loader to a newer version.

  * Plug the SD card into your Beagle.

  * Turn on your Beagle and get to the # prompt -- make sure to hit any key to stop autobooting Linux.

  * Enter the following commands.  This will overwrite the current X-Loader on your Beagleboard and replace it with the X-Loader from your SD card.

```
        OMAP3 beagleboard.org # mmcinit
        OMAP3 beagleboard.org # fatload mmc 0 0x80200000 x-load.bin.ift
        OMAP3 beagleboard.org # nandecc hw
        OMAP3 beagleboard.org # nand erase 0 80000
        OMAP3 beagleboard.org # nand write.i 0x80200000 0 80000
```

  * Power off your Beagle, and power it back on.  Hit a key to prevent autoboot.

  * You should now see that you have a newer version of X-Loader.

```
Texas Instruments X-Loader 1.4.2 (Aug  8 2008 - 16:59:05)
Reading boot sector
Booting from nand


U-Boot 2008.10-rc2 (Oct  2 2008 - 09:02:46)

OMAP3530-GP rev 2, CPU-OPP2 L3-165MHz
OMAP3 Beagle board + LPDDR/NAND
DRAM:  128 MB
NAND:  256 MiB
In:    serial
Out:   serial
Err:   serial
Hit any key to stop autoboot:  0
OMAP3 beagleboard.org #
```

## Section IV.  Setting Up Your Automatic Boot ##

This procedure will set up your Beagleboard to automatically boot to the SD card on power on.

If you are using the 2.6.27 kernel:

  * At the U-Boot prompt, enter:

```
        OMAP3 beagleboard.org # setenv bootargs 'console=ttyS2,115200n8 console=tty0 root=/dev/mmcblk0p2 rootdelay=2 rootfstype=ext3 video=omapfb:vram:2M,vram:4M'
```

If you are using the 2.6.28 kernel (the latest):

  * At the U-Boot prompt, enter:

```
        OMAP3 beagleboard.org # setenv bootargs 'console=ttyS2,115200n8 console=tty0 root=/dev/mmcblk0p2 rw rootfstype=ext3 rootwait omapfb.video_mode=1024x768MR-16@60'
```

In either case, enter:

```
        OMAP3 beagleboard.org # setenv bootcmd 'mmcinit; fatload mmc 0 0x80300000 uImage; bootm 0x80300000'
        OMAP3 beagleboard.org # saveenv
```

  * Power off your Beagle and power it back on.

  * It should now boot up directly into Angstrom on its own.
    * Note:  BE PATIENT.  The first boot of Angstrom takes quite a while (5 minutes or so) to unpack all the various files needed.  After that, it boots quite quickly.  So give it time.

  * When Angstrom boots, you should get the following prompt on your console terminal.  (It has a root account set up with no password):

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

Angstrom 2008.1-test-20080918 beagleboard ttyS2

beagleboard login:
```

  * And on your DVI-D Monitor, you should have a log-in screen.  Enter a new username and password, and Angstrom will set up a new account for you on the Beagle.

## Section V.  Getting Beagle on the Internet ##

If you have a USB to 10/100 or a USB to Wi-Fi interface connected into your USB 2.0 Hub connected into your Beagleboard, then you can put your Beagleboard onto the internet.

**NOTE:  802.11 Wi-Fi is NOT my recommendation.**

Most Wi-Fi Interfaces will NOT work.  To date, I have only found the [Belkin USB Wireless G Adapters](http://catalog.belkin.com/IWCatProductPage.process?Product_Id=179211) to work.  I found the following article very helpful as well:  [How to Use Belkin G in Linux](http://opensource.bureau-cornavin.com/belkin/index.html).

But it is not at all easy, nor reliable that Wi-Fi will work.

  * Shutdown your Beagleboard.

  * Plug the 10/100 or Wi-Fi Adapter into your USB Hub.

  * Turn your Beagleboard back on.  If you are lucky -- and sometimes you will be -- then the Beagleboard should discover and enable the Internet during the boot up process.

  * You can check your console log to see if it found the Internet Adapter.  If you use the Trendnet 10/100, you should see something like this on your console terminal:

```
usb 1-1.3.4: new high speed USB device using musb_hdrc and address 5
usb 1-1.3.4: configuration #1 chosen from 1 choice
eth0: register 'asix' at usb-musb_hdrc-1.3.4, ASIX AX88772 USB 2.0 Ethernet, YOUR_MAC_ADDRESS_HERE
```

  * Log into the Console terminal as root, and try a 'ping google.com' to see if your internet is fully functional.  If you get responses, you're all set. Do control-C to exit ping.

  * If not, and you're using 10/100 Wired Ethernet, try the following.  This will shutdown the ethernet, and then re-attempt to bring it back up.  Usually this will work.

```
         root@beagleboard:~# ifdown eth0
         root@beagleboard:~# ifup eth0
```

  * If you are using Wi-Fi, and you need to enter the ESSID in order to associate with your Access Point, or you have WEP or the like turned on, try the following.  You can omit the wlan0 enc line if you don't use WEP.

```
         root@beagleboard:~# ifdown wlan0
         root@beagleboard:~# iwconfig wlan0 essid YOUR_ESSID_HERE
         root@beagleboard:~# iwconfig wlan0 enc YOUR_WEP_KEY_HERE open
         root@beagleboard:~# ifup wlan0
```


  * Hopefully at this point you will have successfully connected to the Internet.  If not, and you're trying wireless, the [Linux Wireless](http://www.linuxwireless.org/) web page may have some good information.

## Section VI.  The Community ##

When you need help with Beagle, there is an active community out there all the time.  The two main spots to look are:

  * The [Beagleboard Discussion Group](http://groups.google.com/group/beagleboard/topics?gvc=2)

  * The Beagleboard Chat.  Just fire up mIRC, Pidgin, Trillilan, etc. and join the Beagle IRC channel.
    * Connect to irc.freenode.net.
    * /join #beagle
    * See HowToJoinBeagleChatGroup for a more detailed description of how to get logged into the group.

## Section VII.  Things That Likely Won't Work ##

The following are the roadblocks that I have encountered and have yet to get over.

  * Wi-Fi:  It might work for you, but don't spend too long trying.  Wired Ethernet works great.

  * The Sound Driver:  There are significant problems with the Sound Driver to date in Angstrom.  It will work, sometimes, and almost always it will lead to a system crash.  There are rumors that this will be fixed in future revisions of the Beagleboard (Rev C).  There are some improvements if you want to go to a different version of the kernel -- a separate subject.  If you want mplayer to run reliably, use 'mplayer -nosound'.

  * The OMAP SGX Graphics accelerator.  The drivers for this have yet to be released.  So for now, all graphics is done in software using the Cortex-A8 with Neon core only.  But note that the Cortex does quite well at graphics.  So the OMAP 3503 may be enough for general graphics needs (2D).

  * The OMAP Video and Audio accelerator.  The drivers for this have yet to be released.  So for now, all video and audio is done in software using the Cortex-A8 with Neon core only.  But note that the Cortex does quite well at video and audio.  So the OMAP 3503 may be enough for general video and audio needs (VGA and below).

## Section VIII.  Things That are Amazing ##

  * omapfbplay:  This is an amazing video player.  It plays the videos using the Cortex A8 and Neon co-processor.  And it plays them as fast as it can (i.e. faster than real-time if it can).  And it displays a frame rate.  Take a look.  It can run VGA videos at 60 fps.  It can run 720p videos at 24fps or so.

  * Running Linux at 500 MHz for Two Watts.  I have measured the power input (5VDC) to the Beagleboard and it consumes between 1.5 and 2.2 watts for the whole board.  And this is without any power management software etc.  Net result is that you have a full Linux computer on the internet, running about 1/2 the speed of a laptop for two watts.

  * Most things will just work.  I've outlined some problems above.  But in general, fire up an application and it will work.  There are three web browsers included (Firefox, Epiphany and Midori).  There is Pidgin instant messaging included.  etc. etc. etc.   Play around, you'll be amazed.

  * The community is very responsive.  Ask a question and you will get an answer.