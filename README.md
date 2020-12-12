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

This installation is done using Opencore, which at the moment is at 0.6.5, and a image from App Store, which you can also get from olarila.com (credits to them)

## Creation of Image and Replacing EFI folder Step by Step >> 

First we go to olarila.com register and download one of their vanilla images, doesn't matter which, any will do, from El Capitan onwards, any will do. 
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


## Proceeding with the installation of macOS on non-Apple computers

If you have the same laptop as me, then we will need to make some changes on the BIOS. 
Such as: Secure-Boot disabled, VMT-d can go as high as 512MB, which is great, we can leave intel VTD enabled(if you have issues, you can disable it and once the OS is installed, we can enable it if needed). Once that is done, we can save the changes, and reboot. 

 Step 1 >> Plug in the USB Stick onto the USB port, and press the power button. After that, we can start pressing the ESC button so that it will show us the Boot menu. We will then choose the USB stick, and then we will be greeted with Apple Boot option. 

 Step 2 >>  Inside Apple Installation menu: we will choose Disk Utility, and once it opens, we will pick the Tab Devices and then enable Show All Drives. 

 Step 3 >> Choose our main Drive/Disk press Erase and then erase as >> Guid Partition Table, 
File System >> MacOS Extended Journaled = HFS+ . Once the erase is complete, we can close that with the combination of keys Alt + Q . 

 Step 4 >> On the Initial Menu, we will choose the Install macOS BigSur on this machine. 
 We go through the steps, like agreeing to the T&C's choosing the Disk( which we have formatted previously on the Disk Utility) and once it's done, it's time to let the system do its job. It will take a while, so might as well go grab yourself a cup of coffee. Don't worry, the system will reboot a few times, and once it has finished the installation, it will pop the initial screen of choosing your region and creating an account. 


## Post-Install 

# After the installation is done, and we are on the desktop, we will need to make a couple changes, like dropping the EFI folder from the USB Stick onto the EFI partition of the Disk, and removing the verbose mode from the boot screen.  Utilities needed: 

 ESP-Mounter Pro >> https://www.olarila.com/files/Utils/ESP%20Mounter%20Pro.app_v1.9.1.zip
 Plist-Editor Pro >> https://nmac.to/plistedit-pro/  

 Step 1 >> Once we are on the desktop, we will need a little utility called ESP-Mounter Pro. 
You can also use any other tool of your preference, as long as it enables us to mount the EFI.
Open ESP_mounter Pro, go through the initial steps( asking for the helper installation and so on), and it will place a little menubar utility. Click on it, then mount the ESP on the USB stick, and copy that EFI folder into the Desktop. Afterwards, we can unmount the EFI from the USB Stick. 

 Step 2 >> Mount the EFI partition from the main Disk. It should be empty, if not, erase the files/folders inside that partition. Copy the EFI folder from the Desktop onto that partition. 
Open it, and navigate inside, open OC folder, and inside OC folder we will find a file named config.plist. We will need to open that with Plist-Editor Pro. 

 Step 3 >> There are two things we need to edit/change on that file.  
 For those eager to remove all those lines of log from the boot screen, we will navigate to NVRAM >> Add >> 7C436110-AB2A-4BBB-A880-FE41995C9F82 >> and then check on the boot-args to remove the -v that is there. The -v stands for Verbose. Also, it contains two other boot-args which will help to Debug in case of Kernel Panic or the system not booting at all: debug=0x100 and keepsyms=1  >> These two, will stop the log exactly where the error has occurred, impeding the system of rebooting instantly.  Afterwards, you can take a picture and check online through other machine, or even through the same, what can be the solution for it. 
Once you have a full booting system, and you know for sure that everything is working accordingly, you can remove all of this boot-args. 

Once this is done, reboot the machine, and you will no longer see the verbose output. This is very useful for when the installation fails or the system does not start. 

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


## What does this EFI folder contains? 

We'll start with ACPI >> In here we will find our fully patched DSDT. ( credits to Daniel Maldonado for it)
It has the battery patches, the brightness, IRQ, HDA/HDEF, you name it, it's there, including patches to the Network Card DW1560 and full iGPU decoding/acceleration. Also on the same folder, we will have SSDT-Friend.aml 
This one works in conjunction with the CPUFriend.kext, which will contain the frequencies that our CPU will operate. According to the Intel website( ark.intel.com ) the TDP can be as low as 800Mhz, and that's what the SSDT will allows us to achieve, instead of the average 1.3Ghz that Apples provides us. 

On the Bootstrap, we have the Bootstrap.efi file, which is there to prevent the eventuality that for some reason Windows or Linux (in case you choose to have a Dual-Boot machine), replaces our BootX64.efi file from the Boot folder. 

On Drivers folder, we have >> AudioDxe.efi > which as you can guess, allows us to have that Chime at the Beginning of the Boot, HFSPlus.efi which is needed for seeing HFS volumes(ie. macOS Installers and Recovery partitions/images), OpenCanopy.efi which allows us to have a beautiful GUI on the Boot picker, OpenRuntime.efi is replacement for AptioMemoryFix.efi, used as an extension for OpenCore to help with patching boot.efi for NVRAM fixes and better memory management, and last but not least Ps2KeyboardDxe.efi will let us use our keyboard to behave like a real Mac upon booting up, i.e. pressing Command to get into the Bootpicker and therefore choosing the Recovery partition or even Resetting the NVRam. 


Now, let's move on to the kexts folder. Kext means Kernel Extension, and it's like the driver for Windows, but in this case, it's for macOS. 

First one is AirportBrcmFixup.kext, and it's responsible for enabling the Wifi of our Network Card. Usually, in order for this kext to function we need to enable the plugins inside of it through our config.plist, but since the patch has already been added to the DSDT.aml, then it's not necessary, all we need is adding the Kext, it's info.plist and content to the config.plist. 

Next one is AppleALC, and this one is to enable the great sound that Apple provides us. This one is a basic one, add the kext and it's contents through config.plist and done. 

Following one is ApplePS2SmartTouchPad.kext, and is the one who manages the trackpad of our machine. Inside we will have the Plugins folder and inside of it ApplePS2Controller and ApplePS2Keyboard. This Plugins need to be added through the config.plist as well. Don't worry, they have been added already, so they're good to go. Inside the Content folder of this kext, we will have the Resources folder which contains the documentation relevant to the kext. I recommend you give it a read and afterwards you can configure the trackpad to your like through the info.plist. 

AsusNBFnKeys is responsible for managing the FN Keys of the Keyboard. For me it's useful in a way of enabling or disabling the touchpad when I'm typing, and giving the correct function to all the keys. 

BrcmBluetoothInjector.kext as the name indicates, injects the Bluetooth onto the system, however this kext alone does not work. It requires the companion ones:  BrcmFirmwareData.kext and BrcmPatchRAM3.kext. The order of this kexts on the config.plist is also important: 
First comes the BluetoothInjextor, followed by the BrcmFirmwareData and finally BrcmPatchRAM3 


CPUFriend.kext is the companion kext to our SSDT-FRIEND.aml which will lower the frequencies to the bare minimum required > 800Mhz. 

Lilu is the most important kext in the whole lot of kexts, since it contains a series of plugins and patches to enable several functions. More information about Lilu can be found on its GitHub: https://github.com/acidanthera/Lilu


NoTouchID.kext is there to inform the system that we don't have a fingerprint reader, since MacBooks  from 2016 started including a TouchID reader. Therefore it's to tell the system we don't have one, and not creating an error in the system. 

USBMap.kext is related to the USB ports external and internal, which include the Webcam and the Bluetooth, making this Built-in. All the ports are working including the SD Card Reader. It only contains the necessary and activated ones, staying below the 15 port limit imposed by Apple. 

VirtualSMC is a advanced Apple SMC emulator in the kernel. Requires Lilu for full functioning. Think of it as the one that will provide sensors information to the system. Inside of this kext, we will find a folder with Plugins, which contain the SMCBatteryManager, SMCProcessor and the SMCLightSensor . It's not difficult to figure out what each one of those plugins do, right? 

Finally, we have WhateverGreen.kext which is related to the GPU of our machine. It contains all sorts of information about the resolution, the display type, brightness, and so on. 

# You can find plenty of information related to these kexts on Acidanthera's GitHub >> https://github.com/acidanthera 


The last folder on our OC folder is Resources. In here we will find the elements necessary to add that beautiful GUI that we see on the boot picker, and also the Boot Chime that we hear upon starting our machine. 


Config.plist is the file that contains the settings related to our system. As I mentioned before, this file should be open with Plist Editor Pro, which I already provided with a link. 
Before making any changes to it, I recommend you know exactly what you are changing and read the Documentation before. DO NOT, I repeat, DO NOT make any changes that you are unfamiliar with. 

Lastly we have OpenCore.efi, the driver that works alongside Bootx64.efi and boots our machine and allows us to have access to the system. Without it, all of this would be meaningless.  OpenCore.efi will get the information from the config.plist and inject it into the system. 

The other two folders on the root of EFI are Apple, which contains some Apple firmware and other files, which are irrelevant for us to mess with, just leave them, since even if we delete that folder, the system will boot, but it will be created again in no time. So just leave and forget about it.  BOOT contains the BOOTx64.efi driver which as I mentioned before, will work in conjunction with Opencore.efi and boot our system without any errors. 


## And there you have it: A fully working EFI folder for a Asus Zenbook UX303UA >> https://www.asus.com/uk/Laptops/ASUS-ZenBook-UX303UA/

Full list of specifications of the one I own >> https://www.asus.com/uk/Laptops/ASUS-ZenBook-UX303UA/specifications/ 

Big Shoutout to Daniel Maldonado, who is a awesome guy, always very helpful, despite being a seriously busy man, to the Acidanthera team, responsible for most kexts that our machine works and the OpenCore drivers and functionality, CorpNewt who made the script of the CPUFRiend and helped me to understand what I could do to get the best battery life out of this machine which is already 4 and a half years old, but still runs strong and still has a good battery life( I am still able to get around 5 hours out of it, depending on the usage of course, but for web browsing and small tasks, it will do the job), and Apple as well, for creating a beautiful and functional OS. 

My last words: Never thought I would be able of accomplishing such a task like this: having a laptop that is more than capable of running Windows 10, Linux and MacOS. When I first bought it, once I arrived home, my first action was to go through the process of setting up Windows 10 and replacing it with Linux Mint. Which I was more familiar with, even though Windows will never change the way it is. It's just so easy to work with and it doesn't give any challenge at all. Even a child of 4/5 years old, can operate with Windows without any hassle. Linux on the other hand, is for grown-ups :D And I truly enjoy the fluidness of Linux and the way it interacts with the whole computer. However, it lacks the software that is available for Windows and Mac. Nowadays, we will be able to find plenty of applications that can replace the official ones, like Microsoft Office, Photoshop, just to name a couple. With Windows yes, we can have all of these applications as well, but at the same time, we will be open to Malware > Trojans, Virus, Spyware, Adware, RansomWare and so on. Yes, Windows Defender and Microsoft have come a long way and improved a lot the security of the system, but truth being told, is that is far from what it should be. But it's nice seeing these improvements. 
On MacOS, we have the Application and we have the security. It's not that Malware is not targeting MacOS, it's just the way that Apple applies Security settings and measures to the system that prevent any malfunction of it, while at the same time, providing us with eye-catching applications and stability, fluidity and most important, security. There is no such thing as a Virus free Operating System, what exists is common sense, to be aware of the websites and applications we visit or use in our machines. 99% the problem sits in front of the screen ;) 

Have fun and keep Hackintoshing 🤘🤘 