_**THIS PAGE IS OUT OF DATE**_

_**DON'T EXPECT THESE HINTS TO WORK**_

# Introduction #

Several hints on controlling various aspects of the Beagle Board when running Linux.


# Details #

## Switching video output between DVI-D and S-Video ##

Switch to the S-Video output.

```
root@beagleboard:~# echo 'tv' > /sys/class/display_control/omap_disp_control/graphics
root@beagleboard:~#
```

Switch back to the DVI-D.

```
root@beagleboard:~# echo 'lcd' > /sys/class/display_control/omap_disp_control/graphics
root@beagleboard:~#
```

Switch video window 1.

```
root@beagleboard:~# echo 'tv' > /sys/class/display_control/omap_disp_control/video1
root@beagleboard:~#
```


## Disabling framebuffer blanking ##

```
root@beagleboard:~# echo -n 0 > /sys/power/fb_timeout_value
root@beagleboard:~#
```

## Listing USB devices ##

```
root@beagleboard:~# cat /proc/bus/usb/devices

T:  Bus=01 Lev=00 Prnt=00 Port=00 Cnt=00 Dev#=  1 Spd=480 MxCh= 3
B:  Alloc=  0/800 us ( 0%), #Int=  2, #Iso=  0
D:  Ver= 2.00 Cls=09(hub  ) Sub=00 Prot=01 MxPS=64 #Cfgs=  1
P:  Vendor=0000 ProdID=0000 Rev= 2.06
S:  Manufacturer=Linux 2.6.22.1-omap1 ehci_hcd
S:  Product=OMAP-EHCI Host Controller
S:  SerialNumber=ehci-omap.0
C:* #Ifs= 1 Cfg#= 1 Atr=e0 MxPwr=  0mA
I:* If#= 0 Alt= 0 #EPs= 1 Cls=09(hub  ) Sub=00 Prot=00 Driver=hub
E:  Ad=81(I) Atr=03(Int.) MxPS=   4 Ivl=256ms

T:  Bus=01 Lev=01 Prnt=01 Port=00 Cnt=01 Dev#=  2 Spd=480 MxCh= 4
D:  Ver= 2.00 Cls=09(hub  ) Sub=00 Prot=01 MxPS=64 #Cfgs=  1
P:  Vendor=05e3 ProdID=0606 Rev= 7.02
S:  Product=USB2.0 Hub
C:* #Ifs= 1 Cfg#= 1 Atr=e0 MxPwr=100mA
I:* If#= 0 Alt= 0 #EPs= 1 Cls=09(hub  ) Sub=00 Prot=00 Driver=hub
E:  Ad=81(I) Atr=03(Int.) MxPS=   1 Ivl=256ms

T:  Bus=01 Lev=02 Prnt=02 Port=00 Cnt=01 Dev#=  3 Spd=480 MxCh= 0
D:  Ver= 2.00 Cls=ff(vend.) Sub=ff Prot=00 MxPS=64 #Cfgs=  1
P:  Vendor=13b1 ProdID=0018 Rev= 0.01
S:  Manufacturer=
S:  Product=USB 2.0 Network Adapter ver.2
S:  SerialNumber=01D5FE
C:* #Ifs= 1 Cfg#= 1 Atr=a0 MxPwr=250mA
I:* If#= 0 Alt= 0 #EPs= 3 Cls=ff(vend.) Sub=ff Prot=00 Driver=asix
E:  Ad=81(I) Atr=03(Int.) MxPS=   8 Ivl=128ms
E:  Ad=82(I) Atr=02(Bulk) MxPS= 512 Ivl=0ms
E:  Ad=03(O) Atr=02(Bulk) MxPS= 512 Ivl=0ms

T:  Bus=01 Lev=02 Prnt=02 Port=03 Cnt=02 Dev#=  4 Spd=480 MxCh= 0
D:  Ver= 2.00 Cls=02(comm.) Sub=00 Prot=00 MxPS=64 #Cfgs=  2
P:  Vendor=0525 ProdID=a4a2 Rev= 2.16
S:  Manufacturer=Linux 2.6.22.1-omap1/musb_hdrc
S:  Product=RNDIS/Ethernet Gadget
C:  #Ifs= 2 Cfg#= 2 Atr=c0 MxPwr=100mA
I:  If#= 0 Alt= 0 #EPs= 1 Cls=02(comm.) Sub=02 Prot=ff Driver=
E:  Ad=82(I) Atr=03(Int.) MxPS=  16 Ivl=32ms
I:  If#= 1 Alt= 0 #EPs= 2 Cls=0a(data ) Sub=00 Prot=00 Driver=
E:  Ad=81(I) Atr=02(Bulk) MxPS= 512 Ivl=0ms
E:  Ad=01(O) Atr=02(Bulk) MxPS= 512 Ivl=0ms
C:* #Ifs= 2 Cfg#= 1 Atr=c0 MxPwr=100mA
I:* If#= 0 Alt= 0 #EPs= 1 Cls=02(comm.) Sub=06 Prot=00 Driver=cdc_ether
E:  Ad=82(I) Atr=03(Int.) MxPS=  16 Ivl=32ms
I:  If#= 1 Alt= 0 #EPs= 0 Cls=0a(data ) Sub=00 Prot=00 Driver=cdc_ether
I:* If#= 1 Alt= 1 #EPs= 2 Cls=0a(data ) Sub=00 Prot=00 Driver=cdc_ether
E:  Ad=81(I) Atr=02(Bulk) MxPS= 512 Ivl=0ms
E:  Ad=01(O) Atr=02(Bulk) MxPS= 512 Ivl=0ms
root@beagleboard:~# lsusb
Bus 001 Device 004: ID 0525:a4a2 Netchip Technology, Inc. Linux-USB Ethernet/RNDIS Gadget
Bus 001 Device 003: ID 13b1:0018
Bus 001 Device 002: ID 05e3:0606 Genesys Logic, Inc.
Bus 001 Device 001: ID 0000:0000
Bus 1 Device 1: ID 0000:0000
Bus 1 Device 3: ID 13b1:0018
Bus 1 Device 4: ID 0525:a4a2 Netchip Technology, Inc. Linux-USB Ethernet/RNDIS Gadget
Bus 1 Device 2: ID 05e3:0606 Genesys Logic, Inc.
root@beagleboard:~#
```