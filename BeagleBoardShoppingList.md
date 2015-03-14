# New official page #

This page is consistently out-of-date.  The new home page for this information is:
http://beagleboard.org/peripheral

# Introduction #

This is a list of all the items that I needed to create my Beagleboard setup.  I think this will be helpful to others.

# Details #

## Section I:  What You Probably Have Already ##

You likely have all of the below.  And you can use them as-is with the Beagleboard.  If you don't have them, you'll need them.

|Vendor|URL|Part No.|Description|Price|Comments|
|:-----|:--|:-------|:----------|:----|:-------|
|You| - | - |Windows or Linux PC|$0.00|A PC or Laptop either Linux or Windows with a RS-232 Serial port to act as the Beagleboard's console terminal, and to program SD Cards.|
|You| - | - |DVI-D Monitor|$0.00|A monitor with a DVI-D input to connect as the Beagle's Monitor.|
|You| - | - |Keyboard|$0.00|Any old USB keyboard.|
|You| - | - |Mouse|$0.00|Any old USB mouse.|
|You| - | - |RS-232 Null-Modem DB9-F to DB9-F Serial Cable|$0.00|You will need one of these.  If you don't already have one, see the Likely section below.|
|You| - | - |Powered Speakers|$0.00|If you want to listen to the Beagleboard, you'll need a set of amplified speakers.|
|You| - | - |Power Strip|$0.00|The Beagle Board, USB Hub, and Speakers will each need to plug in, and each has a power brick.  So a six-position power strip is a good idea.|
|You| - | - |Patience|Priceless|It takes a long time to burn SD Cards, download images, etc etc.|

## Section II:  The Minimalist Kit:  What You'll Need But Won't Already Have ##

If you are planning to use the Beagleboard as a Host (i.e. with peripherals like keyboard, mouse, ethernet, etc. attached), then here is a list of how to get started easily.  More details are contained in the sections below as well.

In order to be small and inexpensive, the Beagleboard uses some connectors for which you may not already have the proper cabling.  So via use of the adapters below, you can convert your Beagleboard into a standard DVI-D, RS-232 Serial Port, and USB Host.

Important note:  The Beagleboard does not produce the DVI-Analog signals -- so it's DVI-D only.  You cannot connect a VGA monitor to the Beagle -- (without expensive Analog to Digital Converters).  It's not good enough to use a gender changer etc.

Another note:  Take a look at Special Computing's website.  They are selling a package that includes the Beagleboard, case, and some of the cables etc.  I've listed the individual pieces below.

|Vendor|URL|Part No.|Description|Price|Comments|
|:-----|:--|:-------|:----------|:----|:-------|
|DigiKey|http://search.digikey.com/scripts/DkSearch/dksus.dll?Detail?name=296-23428-ND|296-23428-ND|Beagle Board|$149.00|Here's the starting point!  BTW, this board is now also available from Special Computing, Spark Fun, and a few other vendors worldwide.|
|Special Computing|http://www.specialcomp.com/beagleboard| - |USB-Std-A Male to 5.5x2.1mm Barrel Connector|$8.00|This will use up a port on the USB Hub.  But allows the USB Hub's power supply to power the Beagleboard -- along with the other peripherals.  Also available from SparkFun Electronics.|
|Special Computing|http://www.specialcomp.com/beagleboard| - |Acrylic Case for Beagle|$29.00|This will keep your board safe.|
|Special Computing|http://www.specialcomp.com/beagleboard| - |Monitor Cable (HDMI-A-male to DVI-D male)|$11.00|This cable connects the DVI-D Monitor directly to the Beagleboard without any adapter.|
|Special Computing|http://www.specialcomp.com/beagleboard| - |USB Std-A-Female to Mini-A-Male Adapter|$9.00|Adapter:  USB Host.  This gets your Beagleboard to have a USB Standard-A Receptacle so that you can use any standard USB peripheral with the Beagle (including of course a USB Hub).  Note:  The Mini-A OTG Plug is not what you have on any normal USB Cable.  So if you want to plug a USB Hub or other USB Device into the OTG port on the Beagle, then you must have this Mini-A OTG Plug to get it to work.  The normal setup is to have a Mini-B Plug, and that is for connecting the Beagle as a device to another computer.|
|Special Computing|http://www.specialcomp.com/beagleboard| - |DB9M to IDC10F AT/Everex Serial Adapter|$5.00|Adapter:  Serial Port.  This gets your Beagleboard to have a DB9-Male RS-232 Serial port connector that can work with any null-modem serial cable.  You may be able to scavenge one of these AT/Everex Cables out of an old PC.  Make sure that it is "straight through" wiring -- i.e. pin 1 to pin 1, pin 2 to pin 2, etc.|
|Special Computing|http://www.specialcomp.com/beagleboard| - |Null Modem RS-232 DB9F-DB9F Cable|$4.00|A cable to connect the Beagle's serial port to a PC.|
|Special Computing|http://www.specialcomp.com/beagleboard| - |3-port USB Hub with 10/100 Ethernet|$34.00|This will allow keyboard, mouse, 10/100 ethernet, and Beagleboard power cable to connect to Beagleboard.  If you need more USB ports than that, you will need a separate USB Hub and Ethernet adapter (see below).|
|Special Computing|http://www.specialcomp.com/beagleboard| - |USB SD Card Reader|$5.00|SD Card reader for creating SD Cards on your PC.|
|Supermediastore.com|http://www.supermediastore.com/apacer-2gb-60x-secure-digital-sd-card.html|ME-001-1789|Apacer 2GB 60X SD Cards|$8.99|You'll need a handful of SD cards.|

## Section III:  Other Not Mandatory Purchases: ##

|Vendor|URL|Part No.|Description|Price|Comments|
|:-----|:--|:-------|:----------|:----|:-------|
|Tin Can Tools|http://www.tincantools.com/product.php?productid=16147&cat=255&page=1| -  |Zippy Board|$79.00|An add-on board that provides additional peripheral interfaces for the Beagle -- including Ethernet 10/100, SD/MMC, RS-232, Real-time Clock, and I2C. (Requires soldering)|
|Tin Can Tools|http://www.tincantools.com/product.php?productid=16146&cat=251&page=1| - |14-pin to 20-pin JTAG Adapter|$19.00|An adapter that lets you connect an ARM JTAG Debugger to the Beagleboard.|
|DigiKey|http://search.digikey.com/scripts/DkSearch/dksus.dll?Detail?name=AE10074-ND|AE10074-ND|1.5m S-Video (4-pin M-M) Cable|$11.95|A cable to connect to a TV monitor via S-Video.  Obviously other lengths would be ok.|
|DigiKey|http://search.digikey.com/scripts/DkSearch/dksus.dll?Detail?name=DA-70227-ND|DA-70227-ND|7-port USB 2 Hub|$19.69|This will allow up to seven peripherals to connect to the Beagle.  And using the Sparkfun cable above, it will power the Beagleboard as well.|
|Monoprice|http://www.monoprice.com/products/product.asp?c_id=104&cp_id=10404&cs_id=1040401&p_id=173&seq=1&format=2|173|USB Std-A-Male to Dual PS2 Female Adapter|$4.00|Adapter #4:  Keyboard and Mouse.  This gets your Beagleboard to have a Keyboard and Mouse PS2 interface.  You can plug this directly into Adapter #2, or you can use a USB Hub.|
|Monoprice|http://www.monoprice.com/products/product.asp?c_id=102&cp_id=10238&cs_id=1023802&p_id=2696|2696|HDMI-A/Male to M1-D/Male 6 ft. cable|$9.29|This cable will connect your Beagleboard to many different projectors (with an M1 Connector).|
|Monoprice|http://www.monoprice.com/products/product.asp?c_id=104&cp_id=10419&cs_id=1041902&p_id=2080|2080|HDMI-A-male to DVI-D-female Adapter|$3.63|Adapter #1:  DVI Monitor.  This gets your Beagleboard to have a DVI-D Female connector so that any standard DVI-D monitor cable can be used.  If you prefer to buy a single cable, see the HDMI-DVI-D Cable below.|
|Radio Shack|http://www.radioshack.com/product/index.jsp?productId=2769659|CA-2002|Powered Speakers|$22.00|Important that these are amplified speakers.  Other than that, any speakers that would plug into your PC will work.|
|Radio Shack|http://www.radioshack.com/product/index.jsp?productId=2806154|TU2-ET100|Trendnet USB to Ethernet 10/100 Adapter|$25.00|For wired 10/100 internet connections.|
|Staples or elsewhere|http://catalog.belkin.com/IWCatProductPage.process?Product_Id=179211|F5D7050|Belkin USB Wireless G Adapter|$39.00|Wireless drivers are a bit harder to come by.  So this specific model has been tested and works.  Don't buy any other brand!  Drivers are hard to come by.  In fact I tried version 3002.  So different versions of the Belkin may or may not work.|

## Section IV: Powering your Beagleboard ##

If you are going to use your Beagleboard as a device/gadget connected to a laptop, then you can power the Beagleboard through the USB OTG port (using a standard USB A to MiniB cable).  So no external supply will be necessary.

However, if you plan to use your Beagleboard as a USB Host (with peripherals attached to the Beagleboard), then you must supply power to the alternate power jack on the board.  The Alternate Power Jack is a 5.5mm outer diameter x 2.1mm inner diameter jack.  And the board wants 5 VDC with positive tip, and ground on the outer barrel.

Another important note:  The USB OTG Mini-AB receptacle the Beagleboard can only supply 100 mA to devices through the OTG port.  To get more power for a device, it must either be through a powered USB Hub, or through the USB Host Standard-A receptacle.  This means that in all likelihood, the way you will use the Beagleboard is to connect an **externally-powered** (not bus powered) USB Hub to the Beagleboard.  So the USB Hub will supply power to the peripherals.

And last but not least, if the USB Hub is powered, then it can supply power to the Beagleboard as well as the peripherals.  So one interesting setup is to have a single power supply connected to the USB Hub.  Then have all the peripherals, and the Host Beagleboard connected to the USB Hub.  Thus, there would be two cables from the Beagleboard to the USB Hub.  The Powering Cable would run from one of the USB Hub's Standard A Receptacles to the Beagleboard's Alternate Power Barrel Connector.  The Communicating Cable would run from the outbound connection on the USB Hub to either the Beagleboard's Host A Receptacle or to the Beagleboard's Mini-AB OTG Receptacle.  Special Computing has a very nice picture of this setup on their website at:  http://specialcomp.com/beagleboard/RevC0.htm.

So in summary, there are four common ways to power the Beagleboard.

  1. In Gagdet Mode, via the USB OTG connector by connecting a typical USB A Male to USB Mini-B Male cable from your PC to the Beagleboard.  In this mode, the Beagleboard is a device that is connected to and powered by the Host PC.
  1. In Host Mode, via a USB to Barrel connector from a powered USB Hub (see above).
  1. In Host Mode, via a 5V DC power supply purchased with the 5.5 x 2.1 mm Barrel Connector (see Digikey power supply below).
  1. In Host Mode, via a 5V DC power supply by splicing on the appropriate 5.5 x 2.1 mm Barrel Connector (see Digikey pigtail below).

Listed below are sources for the three Host Mode methods.

|Vendor|URL|Part No.|Description|Price|Comments|
|:-----|:--|:-------|:----------|:----|:-------|
|DigiKey|http://search.digikey.com/scripts/DkSearch/dksus.dll?Detail?name=CP-2185-ND|CP-2185-ND|Barrel Connector Pig-Tail for 5V Supply|$1.74|If you have a supply and are up to connecting wires together, this is the proper connector for the Beagleboard Power Jack.  Remember to put +5 on the tip, and Ground on the outer barrel.|
|DigiKey|http://search.digikey.com/scripts/DkSearch/dksus.dll?Detail?name=T450-P5P-ND|T450-P5P-ND|Power Supply for the Beagle|$16.63|If you don't have a Power Supply, or don't want to do any wiring, and if you use the Beagle as a Host, you MUST have a power supply.|

## Section V:  Software for using a Windows PC:  Terminal Emulator, Virtual Machine Software, Linux Distro ##

### NOTE:  If you are using a Linux PC for development, then you don't need the following items.  They will let you use a Windows PC. ###

|Vendor|URL|Part No.|Description|Price|Comments|
|:-----|:--|:-------|:----------|:----|:-------|
|Source Forge|http://sourceforge.jp/projects/ttssh2/files/|TeraTerm|Tera Term Terminal Emulator Software|$0.00|This is the software that you will need to connect with your PC's serial port.  Once booted up in Linux, you can use this software as an SSH Client with your Beagle as well.|
|Vmware|http://www.vmware.com/products/player/|Vmware Player|VMware Player|$0.00|Free Vmware Player software needed to format and load SD Cards.|
|Chrysaor|http://chrysaor.info/|Ubuntu 8.04.1 Desktop|VMware Image of Ubuntu Desktop|$0.00|Free Ubuntu Desktop for VMware Player with 8 GB Hard Drive|
|Jars|http://jars.de/english/ubuntu-804-vmware-image-download-english|Ubuntu 8.04 Desktop|VMware Image of Ubuntu Desktop|$0.00|Free Ubuntu Desktop for VMware Player with 20GB Hard Drive|
|7-Zip|http://www.7-zip.org/download.html|7-Zip File Manager|7-Zip File Manager|$0.00|Free compression / decompression archiving software|
|Cygwin|http://www.cygwin.com/|setup.exe|Cygwin for Windows|$0.00|Free Linux Windows Program|
|Trillian|http://www.ceruleanstudios.com|Trillian Basic|Trillian Basic 3|$0.00|Free IM/Chat Client.  You can use this (or other clients) to connect with the Beagle IRC Chat group (irc.freenode.net /join #beagle).|

## Section VI:  Community Websites: ##

You'll need to peruse each of these sites, collect valuable tidbits and print out many important how-to documents:

|Vendor|URL|Part No.|Description|Price|Comments|
|:-----|:--|:-------|:----------|:----|:-------|
|www|http://beagleboard.org/| - |Beagleboard Home Page|$0.00| - |
|www|http://code.google.com/p/beagleboard/wiki/HowToGetAngstromRunning| - |Beagleboard: From box to Angstrom Distro Cookbook|$0.00| - |
|www|http://code.google.com/p/beagleboard/wiki/BeagleSourceCode| - |Beagleboard Source Code and Binary Images|$0.00| - |
|www|http://code.google.com/p/beagleboard| - |Beagleboard Project Wiki|$0.00| - |
|www|http://code.google.com/p/beagleboard/downloads/list| - |Beagleboard Downloads|$0.00| - |
|www|http://groups.google.com/group/beagleboard| - |Beagleboard Discussion Group|$0.00| - |
|www|http://elinux.org/BeagleBoard| - |Linux on Beagleboard Wiki|$0.00| - |
|www|http://elinux.org/BeagleBoardFAQ| - |Linux on Beagleboard FAQs|$0.00| - |
|www|http://elinux.org/BeagleBoardBeginners| - |Linux on Beagleboard Beginners Guide|$0.00| - |
|www|http://elinux.org/BeagleBoardAndOpenEmbeddedGit| - |Linux on Beagleboard Angstrom Build|$0.00| - |
|www|http://www.codeplex.com/BeagleBoard| - |Windows CE on Beagleboard Wiki|$0.00| - |