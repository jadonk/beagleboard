# Introduction #

The only tricky part of this procedure is that in order for the Beagleboard to be a USB Host, you MUST plug a USB Mini-A Male into the OTG socket on the Beagleboard.  Rest assured that it is very highly unlikely that any of your existing cables (even if they have USB Mini-Male connectors on them) are Mini-A connectors.  Almost all cables are Mini-B and this will put the Beagleboard into Device mode, not Host Mode.

So to solve that problem, there is a USB Standard-A-Female to USB Mini-A-Male Adapter available from a few sources ([www.vernier.com](http://www.vernier.com/accessories/access.html?usb-mini&template=basic.html), [www.serialio.com](https://serialio.com/store/product_info.php?products_id=456)) that effectively converts the Mini-A Socket on the Beagleboard into a Standard-A Socket.

Once you have that, your Beagleboard is now just like any other USB Host.


# Details #

![http://elinux.org/upload/8/8a/Beagle_top_notes.jpg](http://elinux.org/upload/8/8a/Beagle_top_notes.jpg)

## Step 1:  Connect your AT/Everex Cable to the Beagle Board. ##

### BE SURE THAT THE RED STRIPE ON THE RIBBON CABLE IS NEAR PIN 1 OF THE CONNECTOR. ###

This is what the AT/Everex Cable will look like:

![http://www.computergate.com/products/images/tb181/CFIR09D.jpg](http://www.computergate.com/products/images/tb181/CFIR09D.jpg)

The socket end plugs onto the pin header (#10 in the picture below).  It's important to have the red stripe on the ribbon cable (marks pin 1) next to the P9 Pin 1 marking on the board (on the left as shown below).

NB:  Some of the AT/Everex Cables come with a plug in the hole for pin 10.  This is a keying and is actually a good idea (it prevents plugging the cable in backwards).  I suggest that you carefully (with a needle nose pliers) break off pin 10 on the board so that the connectors will mate.  A few wiggles back and forth and pin 10 should be gone.

## Step 2:  Connect your **Null Modem** DB9F to DB9F cable.  One end goes to the AT/Everex cable, the other goes to the serial port on the back of your laptop (aka COM1). ##

## Step 3:  Connect your LCD Panel's digital port to the Beagleboard. ##

This will use the HDMI-M to DVI-D M Cable as shown below.  The HDMI end connects to connector 3 (as shown in the Beagleboard picture above).  Turn on the Monitor.

## Step 4:  Connect your powered Speakers to the Audio Out port. ##

The Audio Out port is lableled #14 in the Beagle photo above.  Plug in the speakers, and turn them on.

## Step 5:  (Optional)  Connect your TV to the Beagle (via S-Video). ##

The S-Video port is #15 in the picture above.  If you're not planning to use a TV with the Beagle you can skip this step.

## Step 6:  Connect your USB Hub and periperals. ##

  * Connect the USB Std-A-Female to USB MIni-A-Male Adapter to the OTG socket on the Beagleboard (shown as #12 in the board picture above).

![http://www.vernier.com/images/products/usb-mini_web.jpg](http://www.vernier.com/images/products/usb-mini_web.jpg)

  * Connect your USB Hub to the Std-A Female end of the adapter.

  * Connect your keyboard and mouse to the USB Hub


## Step 7:  Power up your Beagle! ##

Connect your power supply to Connector 11 in the photo above.  And wait a few seconds.  You should observe the following:

  * In your console terminal window you should see:

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

  * From your speakers, you should hear about a two second tone (kind of annoying).

  * On your DVI-D Monitor you should see the Beagleboard Logo

  * On your TV monitor (if connected), you should see color bars.


### IF YOU GET ALL THESE THINGS, THEN YOU ARE READY TO START LOADING SOFTWARE ONTO SD CARDS FOR USE WITH THE BEAGLE. ###