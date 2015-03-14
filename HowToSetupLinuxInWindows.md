# Introduction #

This is a procedure that shows how to download, and set up a VMware Player along with a Desktop Linux Image so that you can have a complete Linux environment running on a Windows-based PC.  At the end, it points you to the Open Embedded / Angstrom Distro build procedures should you also desire to build Angstrom Images for your Beagleboard.

# Details #

## Step 1:  Download, Install and Configure Teraterm.  This is a freeware terminal emulator for Windows. ##

  * Download Teraterm from http://sourceforge.jp/projects/ttssh2/files/
  * Run the downloaded .exe file to install Teraterm -- default answers are fine.
  * Run Teraterm and Select Serial, COM1 (or whatever COM port you intend to use with the Beagleboard.
  * Do Setup-Window and put Beagleboard Console Port as the Title.
  * Do Setup-Serial Port and set your COM1 port to 115200n81 (as below).
  * Do Setup-General and select English as the language:
  * Do Setup-Save Setup and enter TERATERM.INI as the filename
  * At this point Teraterm should be running and connected to your Beagleboard.


## Step 2:  Download and Install VMware Player.  This is a "virtual machine" application that will allow you to run a Linux PC in a window under Windows. ##

  * Download the VMware Player (it's free) from:  http://www.vmware.com/products/player/
  * Install it (default answers are fine).

## Step 3:  Download and Install the 7-Zip File Manager.  This is a utility that allows up to unpack the Ubuntu Zip file. ##

  * Download from http://www.7-zip.org/download.html (it's free).
  * Install it (default answers are fine).

## Step 4:  Download and Install an Ubuntu Image. ##
  * Download from http://chrysaor.info/?page=ubuntu.  Click Download via HTTP on the latest Ubuntu VMware Desktop Image.  This is about 1GB -- so it will take a long time to download.
  * Once the download is complete, start the 7-Zip File Manager program.
  * Navigate to the folder where you placed the 1GB Ubuntu image file, and you should see the ".7z" file with the Ubuntu image in it.
  * Select the Ubuntu 7z file and click Extract.  This will "unzip" the image into about a 4GB folder when it's done.

## Step 5:  Run VMware and Load the Virtual Machine ##
  * Run VMware Player
  * Click on the Open icon, and navigate to the folder that the Ubuntu Image was unzipped into and click on the .vmx file.  This will start up the Linux PC in the VMware window.  It takes a couple of minutes to load.  So be patient.
  * There is some trickiness to VMware because it needs to decide which devices (keyboard, etc) are connected to which "virtual machine"  (Windows or Linux).
  * To connect your keyboard to Linux do "control-G".  To connect your keyboard to Windows, do "Control-Alt".
  * Log into Ubuntu as user with password user.

## Now you have a Linux machine in the VMware window.  You will need this to program SD cards for the Beagle. ##