# Introduction #

This guide is meant for those looking to create a **dual-partition** card, booting from a FAT partition that can be read by the OMAP3 ROM bootloader and Linux/Windows, then utilizing an ext3 partition for the Linux root file system.

# Details #

Text marked with [.md](.md) shows user input.

### Determine which device the SD Card Reader is on your system ###

Plug the SD Card into the SD Card Reader and then plug the SD Card Reader into your system.  After doing that, do the following to determine which device it is on your system.

```
$ [dmesg | tail]
...
[ 6854.215650] sd 7:0:0:0: [sdc] Mode Sense: 0b 00 00 08
[ 6854.215653] sd 7:0:0:0: [sdc] Assuming drive cache: write through
[ 6854.215659]  sdc: sdc1
[ 6854.218079] sd 7:0:0:0: [sdc] Attached SCSI removable disk
[ 6854.218135] sd 7:0:0:0: Attached scsi generic sg2 type 0
...
```

In this case, it shows up as /dev/sdc (note sdc inside the square brackets above).

### Check to see if the automounter has mounted the SD Card ###

Note there may be more than one partition (only one shown in the example below).

```
$ [df -h]
Filesystem            Size  Used Avail Use% Mounted on
...
/dev/sdc1             400M   94M  307M  24% /media/disk
...
```

Note the "Mounted on" field in the above and use that name in the umount commands below.

### If so, unmount it ###

```
$ [umount /media/disk]
```

### Start fdisk ###

**Be sure to choose the whole device (/dev/sdc), not a single partition (/dev/sdc1).**

```
$ [sudo fdisk /dev/sdc]
```

### Print the partition record ###

So you know your starting point.  **Make sure to write down the number of bytes on the card (in this example, 2021654528).**

```
Command (m for help): [p]

Disk /dev/sdc: 2021 MB, 2021654528 bytes
255 heads, 63 sectors/track, 245 cylinders
Units = cylinders of 16065 * 512 = 8225280 bytes

   Device Boot      Start         End      Blocks   Id  System
/dev/sdc1   *           1         246     1974240+   c  W95 FAT32 (LBA)
Partition 1 has different physical/logical endings:
     phys=(244, 254, 63) logical=(245, 200, 19)
```

### Delete any partitions that are there already ###

```
Command (m for help): [d]
Selected partition 1
```

### Set the Geometry of the SD Card ###

If the print out above does not show 255 heads, 63 sectors/track, then do the following expert mode steps to redo the SD Card:

  * Go into expert mode.

```
Command (m for help): [x]
```

  * Set the number of heads to 255.

```
Expert Command (m for help): [h]
Number of heads (1-256, default xxx): [255]
```

  * Set the number of sectors to 63.

```
Expert Command (m for help): [s]
Number of sectors (1-63, default xxx): [63]
```

  * Now Calculate the number of Cylinders for your SD Card.

```
#cylinders = FLOOR (the number of Bytes on the SD Card (from above) / 255 / 63 / 512 )

So for this example:  2021654528 / 255 / 63 / 512 = 245.79.  So we use 245 (i.e. truncate, don't round).
```

  * Set the number of cylinders to the number calculated.

```
Expert Command (m for help): [c]
Number of cylinders (1-256, default xxx): [enter the number you calculated]
```

  * Return to Normal mode.

```
Expert Command (m for help): [r]
```

### Print the partition record to check your work ###

```
Command (m for help): [p]

Disk /dev/sdc: 2021 MB, 2021654528 bytes
255 heads, 63 sectors/track, 245 cylinders
Units = cylinders of 16065 * 512 = 8225280 bytes

   Device Boot      Start         End      Blocks   Id  System
```

### Create the FAT32 partition for booting and transferring files from Windows ###

```
Command (m for help): [n]
Command action
   e   extended
   p   primary partition (1-4)
[p]
Partition number (1-4): [1]
First cylinder (1-245, default 1): [(press Enter)]
Using default value 1
Last cylinder or +size or +sizeM or +sizeK (1-245, default 245): [+50]

Command (m for help): [t]
Selected partition 1
Hex code (type L to list codes): [c]
Changed system type of partition 1 to c (W95 FAT32 (LBA))
```

### Mark it as bootable ###

```
Command (m for help): [a]
Partition number (1-4): [1]
```

### Create the Linux partition for the root file system ###

```
Command (m for help): [n]
Command action
   e   extended
   p   primary partition (1-4)
[p]
Partition number (1-4): [2]
First cylinder (52-245, default 52): [(press Enter)]
Using default value 52
Last cylinder or +size or +sizeM or +sizeK (52-245, default 245): [(press Enter)]
Using default value 245
```

### Print to Check Your Work ###

```
Command (m for help): [p]

Disk /dev/sdc: 2021 MB, 2021654528 bytes
255 heads, 63 sectors/track, 245 cylinders
Units = cylinders of 16065 * 512 = 8225280 bytes

   Device Boot      Start         End      Blocks   Id  System
/dev/sdc1   *           1          51      409626    c  W95 FAT32 (LBA)
/dev/sdc2              52         245     1558305   83  Linux
```

### Save the new partition records on the SD Card ###

**This is an important step.  All the work up to now has been temporary.**

```
Command (m for help): [w]
The partition table has been altered!

Calling ioctl() to re-read partition table.

WARNING: Re-reading the partition table failed with error 16: Device or resource busy.
The kernel still uses the old table.
The new table will be used at the next reboot.

WARNING: If you have created or modified any DOS 6.x
partitions, please see the fdisk manual page for additional
information.
Syncing disks.
```

### Format the partitions ###

The two partitions are given the volume names LABEL1 and LABEL2 by these commands.  You can substitute your own volume labels.

```
$ [sudo mkfs.msdos -F 32 /dev/sdc1 -n LABEL1]
mkfs.msdos 2.11 (12 Mar 2005)

$ [sudo mkfs.ext3 -L LABEL2 /dev/sdc2]
mke2fs 1.40-WIP (14-Nov-2006)
Filesystem label=
OS type: Linux
Block size=4096 (log=2)
Fragment size=4096 (log=2)
195072 inodes, 389576 blocks
19478 blocks (5.00%) reserved for the super user
First data block=0
Maximum filesystem blocks=402653184
12 block groups
32768 blocks per group, 32768 fragments per group
16256 inodes per group
Superblock backups stored on blocks: 
        32768, 98304, 163840, 229376, 294912

Writing inode tables: done                            
Creating journal (8192 blocks): done
Writing superblocks and filesystem accounting information: 
```

# Bootloader settings #

If you use bootloader U-Boot, use following settings to mount root file system at second partition from kernel:


```
console=ttyS2,115200n8 root=/dev/mmcblk0p2 rw rootdelay=1
```