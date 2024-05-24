# BoxAndroid
DISCLAIMERS (PLEASE READ):

Everything you can find in this thread (binaries, texts, code snippets, etc...) are provided AS-IS and are not part of official Armbian project. For this reason not people from Armbian project nor myself are responsible for misuse or loss of functionality of hardware.
THIS POST explains very well the troubles with TV Boxes and why they are not suitable for everyone
Please don't ask about support or assistance in other non-community forums nor in the official Armbian github repository, instead post your questions in this thread, in the TV Boxes forum section (hardware related) or in the Peer-to-peer support section (general linux/software related).
 

Following the recent thread on LibreElec forum about an unofficial image for rk3229 devices, I would like to make public the work made by me and @fabiobassa about bringing rk322x support to armbian.

The project is now in -> mainline Armbian <- development fork -> here <-

 

This first page and the last 3 or 4 pages of the thread are enough to get up to date with recent developments.

Many useful experiences are scattered through the thread, but the most important things are collected here in the first page, so please read it carefully!

 

Mainline kernel is fully supported and will receive most support in the future. Legacy kernel 4.4 is deprecated, but is kept around only for special purposes.

 

What works:

Should boot and work flawlessy on all boards with RK3228a, RK3228b and RK3229, with either DDR2 and DDR3 memories.
Mainline u-boot
Proprietary OPTEE provided as Trusted Execution Environment (needed for DRAM frequency scaling)
All 4 cores are working
Ethernet
Serial UART (configured at 115200 bps, not 1.5Mbps!)
Thermals, CPU and DRAM frequency scaling
OTG USB 2.0 port (also as boot device!)
EHCI/OHCI USB 2.0 ports
MMC subsystem (including eMMC, SD and sdio devices)
Hardware video acceleration
NAND is available only on legacy kernel. To fully boot from NAND, use the Multitool and its steP-nand installation (instructions are below)
Various WIFI over SDIO are supported (SSV6051P, SSV6256P, ESP8089, Realtek chips, etc...), ssv6256p driver is available only on legacy kernel
Full GPU acceleration
U-boot boot order priority: first the sdcard, then the USB OTG port and eventually the internal eMMC; you can install u-boot (and the whole system) in the internal eMMC and u-boot will always check for images on external sdcard/USB first.
 

Unbrick:

Technically, rockchip devices cannot be bricked. If the internal flash does not contain a bootable system, they will always boot from the sdcard. If, for a reason, the bootable system on the internal flash is corrupted or is unable to boot correctly, you can always force the maskrom mode shorting the eMMC clock pin on the PCB. Here there is the procedure, but you can also google around if you get stuck on a faulty bootloader, the technique is pretty simple and requires a simple screwdriver.

 

There are however some unfortunate cases (expecially newer boards) where shorting the eMMC clock pin is difficult or impossibile, like eMMC or eMCP BGA chips with no exposed pins. In those cases pay double attention when burning something on the internal eMMC/eMCP and always test first the image from the sdcard to be sure it works before burning anything on eMMC/eMCP.

 

Some useful links with pins, pads or procedures for some boards:

Generic procedure for boards with non-BGA eMMC
MXQPRO_V71 - eMCP
H20 - eMCP
ZQ01 - eMCP
 

NAND vs eMMC vs eMCP difference:

RK3228 and RK3229 tv boxes comes with three different flash memory chips: eMMC, NAND and eMCP.

It does not depend upon the market name of the tv box and neither the internal board; manufacturers put whatever they find cheaper when they buy the components.

 

NAND chip is just the non-volatile memory
eMMC chip contains both the non-volatile memory plus a controller.
eMCP chip contains the non-volatile memory, a controller for the non-volatile memory (like eMMC), but also contains a bank of DDR SDRAM memory on the same physical chip.
 

The difference is very important, because eMMC and eMCP are far easier to support at various levels: the controller deals with the physical characteristics of the non-volatile memory, so the software has no to deal with.

NAND chips instead are harder to support, because the software is required to deal with the physical characteristics and non-standard things that depends upon the NAND manufacturer.

 

If you have a NAND chips you're unlucky because mainline kernel currently cannot access it, but also because you need special care and instructions explained later.

 

You can discover if you have a NAND, eMMC or eMCP chip looking on the board are reading the signature on the flash memory chip.

The Multitool (see later) also can detect which chip you have onboard: the program will warn you at startup if you have a NAND chip.

 

NAND bootloader upgrade:

IMPORTANT: don't do this is you have an eMMC or eMCP; skip this paragraph if you are unsure too!

For very expert people who are having issues when (re)booting images, there is the chance to upgrade the bootloader on NAND.

The NAND bootloader is nothing else than a regular idbloader (see official rockchip documentation) but contains some bits to correctly access the data on your flash memory.

Upgrading requires to erase the existing flash content, in the worst case will require you to follow the Unbrick procedure above or restore an older but more compatible bootloader.

If you are not mentally ready to overcome possible further issues, don't do this!

 

The detailed instructions and the binaries are available at this post

 

Multimedia:

Mainline kernel: 3D acceleration is provided by Lima driver and is already enabled. Hardware video decoding: https://forum.armbian.com/topic/19258-testing-hardware-video-decoding-rockchip-allwinner/
Deprecated legacy kernel: multimedia features, like OpenGL/OpenGL ES acceleration, hardware accelerated Kodi, ffmpeg and mpv you can take a look to this post
An effective tutorial from @Hai Nguyen on how to configure a box as a hi-quality music player using an USB audio card, and controlling it via remote control is available in this post
 

Brief explanation about kernel naming:

current kernel is the mainline LTS kernel version, most maintained and tested. This is the suggested version for production devices. If you don't know what to pick, pick this.
legacy kernel (version 4.4) is provided by manufacturer; it is deprecated, unmaintained and not suggested.
edge kernel is the development mainline kernel version, with experimental features and drivers; usually stable but perhaps suitable for production devices.
 

You can switch from one kernel flavour to another using armbian-config or manually via apt.

 

Installation (via SD card):

Building:

You can build your own image follow the common steps to build armbian for other tv boxes devices: when you are in the moment to choose the target board, switch to CSC/TVB/EOL boards and select "rk322x-box" from the list.

 

Download prebuilt images from the following links:

Archive builds (GPG-signed) - https://imola.armbian.com/dl/rk322x-box/archive/
SUGGESTED - Nightly built from trunk each week by Armbian servers (GPG-signed) - https://github.com/armbian/community
Old images provided by me (unsigned and outdated) - https://users.armbian.com/jock/rk322x/armbian/stable
 

Archived/older images:

https://armbian.hosthatch.com/archive/rk322x-box/archive/

 

Multitool:

The Multitool is a small but powerful tool to do quick backup/restore of internal flash, but also burn images and general system rescue and maintenance via terminal or SSH.

Compressed images will be uncompressed on fly.

Multitool - A small but powerful image for RK322x TV Box maintenance (instructions to access via network here)
 

Quick installation instructions on eMMC:

Build or download your preferred Armbian image and a copy of the Multitool;
Burn the Multitool on an SD card; once done, place the Armbian image in images folder of the SD card NTFS partition;
Plug the SD card in the TV box and plug in the power cord. After some seconds the blue led starts blinking and the Multitool appears;
OPTIONAL: you can do a backup of the existing firmware with "Backup flash" menu option;
Choose "Burn image to flash" from the menu, then select the destination device (usually mmcblk2) and the image to burn;
Wait for the process to complete, then choose "Shutdown" from main menu;
Unplug the power cord and the SD card, then replug the power cord;
Wait for 10 seconds, then the led should start blinking and HDMI will turn on. The first time the boot process will take a couple of minutes or more because the filesystem is going to be resized, so be patient and wait for the login prompt.
On first boot you will be asked for entering a password for root user of your choice and the name and password for a regular user
Run sudo rk322x-config and select your board characteristics to enable leds, wifi chips, high-speed eMMC, etc...
Run sudo armbian-config to configure timezone, locales and other personal options
Congratulations, Armbian is now installed and configured!
 

Despite the procedure above is simple and reliable, I always recommend to first test that your device boots Armbian images from SD Card.

Due to the really large hardware variety, there is the rare chance that the images proposed here may not boot. If a bad image is burned in eMMC, the box may not boot anymore forcing you to follow the unbrick section at the top of this post.

 

Quick installation instructions on NAND:

Build or download your preferred Armbian image and a copy of the Multitool;
Burn the Multitool on an SD card; once done, place the Armbian legacy kernel image in images folder of the SD card NTFS partition;
Plug the SD card in the TV box and plug in the power cord. After some seconds the blue led starts blinking and the Multitool appears;
OPTIONAL: you can do a backup of the existing firmware with "Backup flash" menu option;
Choose "Burn Armbian image via steP-nand" from the menu, then select the destination device (usually rknand0) and the image to burn;
Wait for the process to complete, then choose "Shutdown" from main menu;
Unplug the power cord and the SD card, then replug the power cord;
Wait for 10 seconds, then the led should start blinking and HDMI will turn on. The first time the boot process will take a couple of minutes or more because the filesystem is going to be resized, so be patient and wait for the login prompt.
On first boot you will be asked for entering a password for root user of your choice and the name and password for a regular user
Run sudo rk322x-config and select your board characteristics to enable leds, wifi chips, etc...
Run armbian-config to configure timezone, locales and other personal options
Congratulations, Armbian is now installed!
 

Alternative: you can install the bootloader in NAND and let it boot from SD Card or USB:

Download a copy of the Multitool and burn it on an SD card;
Plug the SD card in the TV box and plug in the power cord. After some seconds the blue led starts blinking and the Multitool appears;
RECOMMENDED: make a backup of the existing firmware with "Backup flash" menu option;
Choose "Install Jump Start for Armbian" menu option: the Jump Start uses the internal NAND to boot from external SD Card or external USB Stick;
Follow the general instructions to boot from SD Card below, skip the first erase eMMC step.
 

Quick installation instructions to boot from SD Card:

If you are already running Armbian from eMMC, skip to the next step. Instead if you are running the original firmware you need to first erase the internal eMMC; to do so download the Multitool, burn it on an SD Card, plug the SD Card and power the TV Box. Use "Backup flash" if you want to do a backup of the existing firmware, then choose "Erase flash" menu option.
Build or download your preferred Armbian image;
Uncompress and burn the Armbian image on the SD Card;
Plug the SD Card in the TV Box and power it on;
Wait for 10 seconds, then the led should start blinking and HDMI will turn on. The first time the boot process will take a couple of minutes or more because the filesystem is going to be resized, so be patient and wait for the login prompt;
On first boot you will be asked for entering a password for root user of your choice and the name and password for a regular user
Run sudo rk322x-config and select your board characteristics to enable leds, wifi chips, high-speed eMMC or NAND, etc...
Run armbian-config to configure timezone, locales and other personal options, or also to transfer the SD Card installation to internal eMMC;
Congratulations, Armbian is running from SD Card!
 

A note about boot device order:

With Armbian also comes mainline U-boot. If you install Armbian or just the bootloader in the eMMC or the Jump Start on internal NAND, the bootloader will look for valid bootable images in this order:

External SD Card
External USB Stick in OTG Port
Internal eMMC
 

Installation (without SD card, board with eMMC)

If you have no sd card slot and your board has an eMMC, you can burn the armbian image directly on the internal eMMC using rkdeveloptool and a male-to-male USB cable:

 

Download your preferred Armbian image from Armbian download page and decompress it.
Download the rk322x bootloader: rk322x_loader_v1.10.238_256.bin
Download a copy of rkdeveloptool: a compiled binary is available in the official rockchip-linux rkbin github repository.
Unplug the power cord from the tv box
Plug an end of an USB Male-to-male cable into the OTG port (normally it is the lone USB port on the same side of the Ethernet, HDMI, analog AV connectors) while pressing the reset microbutton with a toothpick. You can find the reset microbutton in a hole in the back of the box, but sometimes it is hidden into the AV analog jack
Plug the other end of the USB Male-to-male cable into an USB port of your computer
If everything went well, run lsusb: you should see a device with ID 2207:320b
Run sudo rkdeveloptool rd 3 (if this fails don't worry and proceed to next step)
Run sudo rkdeveloptool db rk322x_loader_v1.10.238_256.bin
Run sudo rkdeveloptool wl 0x0 image.img (change image.img this with the real Armbian image filename)
Unplug the power cord
Done!
 

Installation (without SD card, board with NAND)

If you are in the unfortunate case you can't use an SD card for installation and your board has a NAND chip, you still have an option to use the quick Multitool installation steps via USB.

 

Obtain a copy of rkdeveloptool: a compiled binary is available in the official rockchip-linux rkbin github repository.
Unplug the power cord from the tv box
Plug an end of an USB Male-to-male cable into the OTG port (normally it is the lone USB port on the same side of the Ethernet, HDMI, analog AV connectors) while pressing the reset microbutton with a toothpick. You can find the reset microbutton in a hole in the back of the box, but sometimes it is hidden into the AV analog jack
Plug the other end of the USB Male-to-male cable into an USB port of your computer
If everyting went well, using lsusb you should see a device with ID 2207:320b
Run sudo rkdeveloptool wl 0x4000 u-boot-main.img (download u-boot-main.img.xz , don't forget to decompress it!)
Unplug the power cord
 

Now you can follow the instructions on how to install on eMMC/NAND via SD card, just use instead an USB stick to do all the operations and plug it into the USB OTG port. Once you reboot, USB OTG port will be used as a boot device.

 

NOTE: NAND users without SD slot may be unhappy to know that it will be difficult to do extra maintenance with Multitool in case something breaks in the installed Armbian system: installing u-boot-main.img makes the installed system unbootable because it is missing the NAND driver.

 

 

Alternative backup, restore and erase flash for EXPERTS:

These backup, restore and erase flash procedures are for experts only. They are kept here mostly for reference, since the Multitool is perfectly able to do same from a very comfy interface and is the suggested way to do maintenance.

 

Backup:

Obtain a copy of rkdeveloptool: a compiled binary is available in the official rockchip-linux rkbin github repository. If you prefer, you can compile it yourself from the sources available at official rockchip repository
Unplug the power cord from the tv box
Plug an end of an USB Male-to-male cable into the OTG port (normally it is the lone USB port on the same side of the Ethernet, HDMI, analog AV connectors) while pressing the reset microbutton with a toothpick. You can find the reset microbutton in a hole in the back of the box, but sometimes it is hidden into the AV analog jack
Plug the other end of the USB Male-to-male cable into an USB port of your computer
If everyting went well, using lsusb you should see a device with ID 2207:320b
change directory and move into rkbin/tools directory, run ./rkdeveloptool rfi then take note of the FLASH SIZE megabytes (my eMMC is 8Gb, rkdeveloptool reports 7393 megabytes)
run ./rkdeveloptool rl 0x0 $((FLASH_SIZE * 2048)) backup.data (change FLASH_SIZE with the value you obtained the step before)
once done, the internal eMMC is backed up to backup.data file
 

Restore: first we have to restore the original bootloader, then restore the original firmware.
Running rkdeveloptool with these switches will accomplish both the jobs:

./rkdeveloptool db rk322x_loader_v1.10.238_256.bin
Downloading bootloader succeeded.

./rkdeveloptool ul rk322x_loader_v1.10.238_256.bin
Upgrading loader succeeded.

./rkdeveloptool wl 0x0 backup.data
Write LBA from file (100%)
Download here:

 

Erase the flash memory: clearing the internal eMMC/NAND memory makes the SoC look for external SD Card as first boot option.

If there isn't any suitable SD Card, the SoC enters maskrom mode, which can then be used for full eMMC/NAND access using rkdeveloptool. This is perfectly fine if your box has an eMMC flash memory.

NOTE: In case you have a NAND flash memory this option is however discouraged. The original bootloader contains some special parameters to correctly access the data. Clearing the flash memory will probably garbage the NAND data and restoring the bootloader may require some special instructions.

 

Obtain a copy of rkdeveloptool: a compiled binary is available in the official rockchip-linux rkbin github repository. If you prefer, you can compile it yourself from the sources available at official rockchip repository
Unplug the power cord from the board
Plug an end of an USB Male-to-male cable into the OTG port (normally it is the lone USB port on the same side of the Ethernet, HDMI, analog AV connectors) while pressing the reset microbutton with a toothpick. You can find the reset microbutton in a hole in the back of the box, but sometimes it is hidden into the AV analog jack
Plug the other end of the USB Male-to-male cable into an USB port of your computer
If everyting went well, using lsusb you should see a device with ID 2207:320b
run ./rkdeveloptool ef and wait a few seconds
once done, the internal eMMC is erased and the device will boot from the sdcard from now on
 

Partecipation and debugging:

If you want to partecipate or need help debugging issues, do not hesitate to share your experience with the installation procedure of the boxes.

In case of issues and missed support, provide as many as possible of these things is very useful to try and bring support for an unsupported board:

 

some photos of both sides of the board. Details of the eMMC, DDR and Wifi chips are very useful!
upload the device tree binary (dtb) of your device. We can understand a lot of things of the hardware from that small piece of data; and alternative is a link to the original firmware (you can do a full backup with the Multitool);
dmesg and other logs (use armbianmonitor -u that automatically collects and uploads the logs online)
attach a serial converter to the device and provide the output of the serial port;
 

Critics, suggestions and contributions are welcome!

 

Credits:

@fabiobassa for his ideas, inspiration, great generosity in giving the boards for development and testing. The project of bringing rk322x into armbian would not have begun without his support!
Justin Swartz, for his work and research to bring mainline linux on rk3229 (repository here)
@knaerzche for his great contribution to libreelec support and mainline patches
@Alex83 for his patience in testing the NAND bootloader upgrade procedure on his board
@Jason Duhamell for his generous donation that allowed researching eMCP boards and esp8089 wifi chip
