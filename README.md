# Zenhack
## Hackintosh on Zenbook UX303UA

This project is based on installing macOS on a Asus Zenbook UX303UA, 
with the following specs: 

CPU: i7-6500U;
GPU: Intel HD520 with 1GB VRAM( No dGPU);
12GB 1600Mhz RAM;
500GB SSD Samsung EVO;
Network: DW1560 >> BCM94352Z;
Sound Card: Conexant CX20751/2;
Screen: 1920x1080p;
Touchpad: Focaltech

What works? 
Everything, gestures included which can be configured through the info.plist found inside the kext( at the moment of this guide, Touchpad is working with the assistance of ApplePS2SmartTouchPad.kext which was built by EmlyDinesh and dates to 2017, which as you reckon is outdated, but still works luckily. I am however, waiting on a port of the Linux driver and once I have it, I will update the EFI folder and kexts. 

This installation is done using Opencore, which at the moment is at 0.6.5, and a image from App Store, which you can also get from olarila.com ( credits to Daniel)

## Creation of Image and Replacing EFI folder Step by Step >> 

First we go to olarila.com, register and download one of their vanilla images, doesn't matter which, any will do, from El Capitan onwards, any will do. 
Secondly, depending on which format we download the image, we will need it in .dmg( the usual format used by Apple). If it comes in RAW, we just need to change the extension to .dmg
Third, we use Balena Etcher, which works great under Windows, Linux and macOS. 
We're gonna need a USB Stick of at least 16GB minimum. It needs to be formatted into GPT = Guid Partition Table. 
Once that is done, we can open Balena Etcher, choose flash from file, and point into our macOS image in .dmg format. 
We choose our previously formatted USB Stick, and press flash. It will ask for your password, type it and press Enter. 
It will start flashing the image. If it gives any errors, I recommend trying a different version of BalenaEtcher. You can find them in the releases section. 
Once that is done, it's time to move our EFI into the EFI PArtition of the USB Stick. 
For that, we're gonna need a couple tools: in Windows >>  

## Part 1. Mounting the EFI partition 

Step 1 >> In order to be able to access the GPT/macOS EFI partition on Windows we first need to mount it. There are a couple ways to mount an EFI partition, in my opinion the simplest way is to give it a drive letter.
Step 2 >> Install MiniTool Partition Wizard Free Edition >> https://www.partitionwizard.com/free-partition-manager.html
Step 3 >> Open MiniTool Partition Wizard
Step 4 >> Right-Click the EFI partition and click Change Letter
Step 5 >> Select an unused drive letter (I picked Z:)
Step 6 >> Click Apply
Step 7 >> Apply pending Changes? : Yes
Step 8 >> Click OK

## Part 2. Access & Edit EFI partition on Windows
In order to open the EFI partition on Windows and be able to make changes to it’s contents we are going to use a third party program called Explorer++

Step 1 >> Download Explorer++ >> https://explorerplusplus.com/
Step 2 >> Unzip explorer++.zip by right-clicking the zip file and selecting Extract All… and click Extract
Step 3 >> Open the now unzipped explorer++ folder
Step 4 >> Right-Click Explorer++ and select Run as administrator
Step 5 >> You should now see your EFI folder which you can open and make changes too through the Explorer++ program.

Note: If you're trying to change/modify your EFI folder after you just flashed the image onto the USB, it is possible that you may not see the EFI folder/Partition. 
If that's the case just reboot your machine without the USB and afterwards, repeat the Part 1 and 2 of this guide.

Once that is done, you can then replace the EFI folder on that EFI partition with the one provided on this link. 



The config.plist found inside the OC folder, does not contain a Serial, nor SystemUUID neither MLB. You will need to create new ones using the script provided by Corpnewt >> https://github.com/corpnewt/GenSMBIOS
Instructions on how to do that are simple: 
For our Skylake laptop, we'll choose the MacBookPro13,1 SMBIOS. 
Run GenSMBIOS, pick option 1 for downloading MacSerial and Option 3 for selecting out SMBIOS. This will give us an output similar to the following:

 #               MacBookPro13,1 SMBIOS Info            #
#######################################################

Type:         MacBookPro13,1
Serial:       C02S3HYWGG7L
Board Serial: C02629102GUGPF7AD
SmUUID:       3508AD44-B67D-4AD7-A109-7955130A1033

The Type part gets copied to Generic -> SystemProductName.

The Serial part gets copied to Generic -> SystemSerialNumber.

The Board Serial part gets copied to Generic -> MLB.

The SmUUID part gets copied to Generic -> SystemUUID.

We set Generic -> ROM to either an Apple ROM (dumped from a real Mac), your NIC MAC address, or any random MAC address (could be just 6 random bytes, for this guide we'll use 11223300 0000. After install follow the Fixing iServices page on how to find your real MAC Address >> https://dortania.github.io/OpenCore-Post-Install/universal/iservices.html)

# Will finish this later, some family issues at the moment, which are more important
