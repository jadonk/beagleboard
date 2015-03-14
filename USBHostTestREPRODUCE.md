# Introduction #

USB Host Validation on Beagle

# Details #

**Procedure to get source for Kernel, u-boot and X-loader to execute these tests**

> - Download :
    * x-loder : _http://beagleboard.googlecode.com/files/x-load-beagle-rev2.tar.gz.gz_
    * u-boot  : _http://beagleboard.googlecode.com/files/u-boot-beagle-rev2-trial2.tar.gz.gz_
    * kernel  : _http://www.beagleboard.org/uploads/2.6\_kernel-beagle-rev2.tar.gz_

> - Download source patch:
    * http://beagleboard.googlecode.com/files/usb_validation_src_patches.zip

Apply the patches by executing the patch command in respective directories.

```
patch -d 2.6_kernel -p2 < usb_validation_kernel_patch.diff
patch -d u-boot -p2 < usb_validation_uboot_patch.diff
patch -d x-load -p2 < usb_validation_xload_patch.diff
```

Refer to :
_http://code.google.com/p/beagleboard/wiki/BeagleSourceCode_ for Compiling sources

_http://code.google.com/p/beagleboard/wiki/LinuxBootDiskFormat_ for MMC/SD Card formatting instructions

**VALIDATION SCENARIOS**

_Common Setup_

> - Pre-built images for these tests can be found in here:

> http://beagleboard.googlecode.com/files/usb_validation.zip

> Un-Zip the file in MMC/SD card.

> - Connect a Self Powered HUB to USB HOST connector on Beagle Board

> - Connect a USB HUB to USB HOST connector on Beagle Board

> - Insert the MMC/SD Card (with u-boot.bin, x-loader.bin, uImage and rd-ext2.bin)

NOTE: Refer to MMC/SD card formatting instructions for beagle here,


**TEST 1: Mass Storage device connected USB Host**

_Setup_

> - Connect a USB Mass Storage Device (thumb drive) to USB HUB

> - Power Beagle board externally with a 5 Volt adapter.


_Test Procedure_
> - On Kernel Prompt execute following instructions:
```
        # mount /dev/sda1 /mnt/sda1/
```
> - Try to create a large file on USB device
```
        # dd if=/dev/zero of=/mnt/sda1/myfile bs=1024 count=10240
```

_Failure Observed_
```
     <6>usb 1-1.6: reset high speed USB device using ehci-omap and address 3
     usb 1-1.6: reset high speed USB device using ehci-omap and address 3
     <3>usb 1-1.6: device descriptor read/64, error -71
     usb 1-1.6: device descriptor read/64, error -71
     <3>usb 1-1.6: device descriptor read/64, error -71
     usb 1-1.6: device descriptor read/64, error -71
     <6>usb 1-1.6: reset high speed USB device using ehci-omap and address 3
     usb 1-1.6: reset high speed USB device using ehci-omap and address 3
     <3>usb 1-1.6: device descriptor read/64, error -71
     usb 1-1.6: device descriptor read/64, error -71
     <3>usb 1-1.6: device descriptor read/64, error -71
     usb 1-1.6: device descriptor read/64, error -71
     <6>usb 1-1.6: reset high speed USB device using ehci-omap and address 3
     usb 1-1.6: reset high speed USB device using ehci-omap and address 3
```



**TEST 2: Keyboard / Mouse**

_Setup_

> - Connect a USB Keyboard or Mouse to USB HUB

> - Power Beagle board externally with a 5 Volt adapter.


_Test Procedure_
> - Boot the Linux Kernel

_Failure Observed_
```
Please press Enter to activate this console. <3>ehci-omap ehci-omap.0: port 1 re
set error -110
     ehci-omap ehci-omap.0: port 1 reset error -110
     <3>hub 1-0:1.0: hub_port_status failed (err = -32)
     hub 1-0:1.0: hub_port_status failed (err = -32)
     <3>ehci-omap ehci-omap.0: port 1 reset error -110
     ehci-omap ehci-omap.0: port 1 reset error -110
     <3>hub 1-0:1.0: hub_port_status failed (err = -32)
     hub 1-0:1.0: hub_port_status failed (err = -32)
     <3>ehci-omap ehci-omap.0: port 1 reset error -110
     ehci-omap ehci-omap.0: port 1 reset error -110
     <3>hub 1-0:1.0: hub_port_status failed (err = -32)
     hub 1-0:1.0: hub_port_status failed (err = -32)
     <3>ehci-omap ehci-omap.0: port 1 reset error -110
     ehci-omap ehci-omap.0: port 1 reset error -110
     <3>hub 1-0:1.0: hub_port_status failed (err = -32)
     hub 1-0:1.0: hub_port_status failed (err = -32)
     <3>ehci-omap ehci-omap.0: port 1 reset error -110
     ehci-omap ehci-omap.0: port 1 reset error -110
     <3>hub 1-0:1.0: hub_port_status failed (err = -32)
     hub 1-0:1.0: hub_port_status failed (err = -32)
     <3>hub 1-0:1.0: Cannot enable port 1.  Maybe the USB cable is bad?
     hub 1-0:1.0: Cannot enable port 1.  Maybe the USB cable is bad?
     <3>ehci-omap ehci-omap.0: port 1 reset error -110
     ehci-omap ehci-omap.0: port 1 reset error -110
     <3>hub 1-0:1.0: hub_port_status failed (err = -32)
     hub 1-0:1.0: hub_port_status failed (err = -32)
     <3>ehci-omap ehci-omap.0: port 1 reset error -110
     ehci-omap ehci-omap.0: port 1 reset error -110
     <3>hub 1-0:1.0: hub_port_status failed (err = -32)
```


**TEST 4: Ethernet**


**TEST 5: Connect Mass Storage directly with out an HUB in between**

_Setup_

> - Connect a USB Mass Storage Device (thumb drive) to USB HUB

> - Power Beagle board externally with a 5 Volt adapter.

_Test Procedure_
```
     Same as Test 1
```

_Failure Observed_
```
     Same as Test 1
```



**TEST 6: Connect Keyboard, Mouse and Ethernet adapter with an HUB**

_Setup_

> - Connect a USB Mouse, Keyboard, Ethernet to USB HUB

> - Power Beagle board externally with a 5 Volt adapter.

_Test Procedure_
```
     Same as Test 1
```

_Failure Observed_
```
     Same as Test 1
```



**TEST 7: Send ULPI test packets capture them with CATC**

_Setup_

> - Connect a CATC to USB HOST connector on Beagle.

> - No other device in between

> - Power Beagle board externally with a 5 Volt adapter.

_Test Procedure_
> - When Kernel boots to prompt, execute the following command

```
     # echo "t-pkt" > /sys/devices/platform/ehci-omap.0/port1
```

_Failure Observed_
```
     - For 2097055 packets transmitted
     - The CATC trace shows 
          - Bad PID           : 29
          - Bad CRC16         : 257
          - Bad Packet Length : 29

Note: On another OMAP3 board with different PHY, with similar test, we don't see any errors
```



**TEST 8: Try writing continously to the SMSC PHY Scratch register using ULPI commands**

This test writes continuously into SMSC Scratch register and reads the content back, if the content matches then it just prints the count otherwise it prints the message as error and prints the expected value and the value returned.

_Setup_

> - Just boot the Kernel

_Test Procedure_
```
     echo "t-force" > /sys/devices/platform/ehci-omap.0/port1
```

_Failure Observed_
```
      TEST PHY READ WRITE
     
      TEST PHY READ WRITE
     0
     0
     1
     1
<so on>
     
     19
     19 : Error! expected  8 returned  0
      : Error! expected  8 returned  0
     
     20
     20
<so on>
     
     1057 : Error! expected  2 returned  0
      : Error! expected  2 returned  0
     
     1058
     1058 : Error! expected  4 returned  0
      : Error! expected  4 returned  0
     
     1059
     1059 : Error! expected  8 returned  0
      : Error! expected  8 returned  0
     
     1060
     1060 : Error! expected 10 returned  0
      : Error! expected 10 returned  0
     
     1061
     1061 : Error! expected 20 returned  0
      : Error! expected 20 returned  0

<so on>
```

NOTE: Due to Debugging enabled in kernel we see counts printed twice.


**TEST 9: Change Pull UP/Down on STP pin of OMAP3 USB HOST**

Changing the Pull up /Pull down settings of STP signal on USB EHCI PINs and then accessing the PHY scratch register as given in TEST 8 results in HANG.

_Setup_

> - Edit _board/omap3530beagle/omap3530beagle.c_ in u-boot.

> - Modify the STP and CLOCK Pins of USB EHCI pins by changing the

> - PTD to PTU,
> - DIS to EN
```
     MUX_VAL(CP(ETK_CLK_ES2),    (IDIS | PTD | DIS  | M3)) /*HSUSB1_STP*/\

NOTE:
      - PTD means Pull Type Down
      - PTU means Pull Type UP
      - DIS disables the Pull Up/Down
      - EN enables the Pull Up/Down
```

> - Re-Compile the u-boot

> - Copy the u-boot.bin in MMC/SD Card

> - Power Beagle board externally with a 5 Volt adapter.

_Test Procedure_

On Kernel prompt execute the following

```
     echo "t-force" > /sys/devices/platform/ehci-omap.0/port1
```

_Failure Observed_
```
      TEST PHY READ WRITE
     
      TEST PHY READ WRITE
     0
     0
     1
     1
     2
     2
     3
     3
     4
     4
     5
     5

<HANGS>

```

NOTE: Due to Debugging enabled in kernel we see counts printed twice.



**TEST 10: Disable STP Pull UP/Down in PHY Interface control register**

Enabling/Disabling Pull up resistor on STP signal in SMSC PHY and then accessing the PHY scratch register as given in TEST 8 results in HANG.

_Setup_

> - Edit _drivers/usb/host/ehci-omap.c_ in Kernel.

> - On line 361, Change PHY\_STP\_PULLUP\_ENABLE to PHY\_STP\_PULLUP\_DISABLE
```
              /*
     353          * The PHY register 0x7 - Interface Control register is
     354          * configured to disable the integrated STP pull-up resistor
     355          * used for interface protection.
     356          */
     357         omap_writel((0x7 << EHCI_INSNREG05_ULPI_REGADD_SHIFT) |/* interface reg      */
     358                 (2 << EHCI_INSNREG05_ULPI_OPSEL_SHIFT) |/*   Write */
     359                 (1 << EHCI_INSNREG05_ULPI_PORTSEL_SHIFT) |/* Port1 */
     360                 (1 << EHCI_INSNREG05_ULPI_CONTROL_SHIFT) |/* Start */
     361                 (PHY_STP_PULLUP_ENABLE),
     362                 EHCI_INSNREG05_ULPI);

```

_Test Procedure_

On Kernel prompt execute the following

```
     echo "t-force" > /sys/devices/platform/ehci-omap.0/port1
```

_Failure Observed with Combination of TEST 8,9 and 10_

```
                          STP in OMAP        STP in PHY           Result
 
Test 1                     Pull Down          Pull Up         Errors / No Hang
 
Test 2                      Pull Up           Pull Up         No Errors / Hangs
 
Test 3                     Pull Down         Pull Down        Errors / No Hang
 
Test 4                      Pull Up          Pull Down        Errors / No Hang
 
Test 5                      DISABLE          Pull Down        Errors / No Hang
 
Test 6                      DISABLE           Pull Up         Errors / No Hang 
 
```