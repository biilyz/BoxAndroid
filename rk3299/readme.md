	<strong>DISCLAIMERS (PLEASE READ):</strong>
</p>

<ul>
	<li>
		Everything you can find in this thread (binaries, texts, code snippets, etc...) are<strong> provided AS-IS and are not part of official Armbian project</strong>. For this reason <strong>not people from Armbian project nor myself are responsible for misuse or loss of functionality of hardware</strong>.
	</li>
	<li>
		<strong><a href="https://forum.armbian.com/topic/12656-csc-armbian-for-rk322x-tv-boxes/?do=findComment&amp;comment=170521" rel="">THIS POST</a></strong> explains very well the troubles with TV Boxes and why they are not suitable for everyone
	</li>
	<li>
		Please don't ask about support or assistance in other non-community forums nor in the official Armbian github repository, instead post your questions in this thread, in the TV Boxes forum section (hardware related) or in the Peer-to-peer support section (general linux/software related).
	</li>
</ul>

<p>
	 
</p>

<p>
	Following the recent thread on <a href="https://forum.libreelec.tv/thread/21117-unoffical-le-9-2-images-for-rk3229/" rel="external nofollow">LibreElec forum</a> about an unofficial image for <abbr title="Rockchip"><abbr title="Rockchip">rk</abbr></abbr>3229 devices, I would like to make public the wo<abbr title="Rockchip"><abbr title="Rockchip">rk</abbr></abbr> made by me and <a contenteditable="false" data-ipshover="" data-ipshover-target="https://forum.armbian.com/profile/13603-fabiobassa/?do=hovercard" data-mentionid="13603" href="https://forum.armbian.com/profile/13603-fabiobassa/" rel="">@fabiobassa</a> about bringing <abbr title="Rockchip"><abbr title="Rockchip">rk</abbr></abbr>322x support to armbian.
</p>

<p>
	The project is now in <strong>-&gt;</strong> <strong><a href="https://github.com/armbian/build" rel="external nofollow">mainline Armbian</a></strong> <strong>&lt;- </strong>development fork <strong>-&gt; <a href="https://github.com/paolosabatino/armbian-build" rel="external nofollow">here</a> &lt;-</strong>
</p>

<p>
	 
</p>

<p>
	<strong>This first page</strong> and the <strong>last 3 or 4 pages</strong> of the thread are enough to get up to date with recent developments.
</p>

<p>
	Many useful experiences are scattered through the thread, but the most important things are collected here in the first page, so please read it carefully!
</p>

<p>
	 
</p>

<p>
	Mainline kernel is fully supported and will receive most support in the future. Legacy kernel 4.4 is deprecated, but is kept around only for special purposes.
</p>

<p>
	 
</p>

<p>
	<strong>What wo<abbr title="Rockchip"><abbr title="Rockchip">rk</abbr></abbr>s:</strong>
</p>

<ul>
	<li>
		Should boot and wo<abbr title="Rockchip"><abbr title="Rockchip">rk</abbr></abbr> flawlessy on all boards with <strong><abbr title="Rockchip"><abbr title="Rockchip">RK</abbr></abbr>3228a,</strong> <strong><abbr title="Rockchip"><abbr title="Rockchip">RK</abbr></abbr>3228b</strong> and <strong><abbr title="Rockchip"><abbr title="Rockchip">RK</abbr></abbr>3229,</strong> with either <strong>DDR2</strong> and <strong>DDR3</strong> memories.
	</li>
	<li>
		<strong>Mainline u-boot</strong>
	</li>
	<li>
		<strong>Proprietary OPTEE</strong> provided as Trusted Execution Environment (needed for DRAM frequency scaling)
	</li>
	<li>
		<strong>All 4 cores</strong> are wo<abbr title="Rockchip"><abbr title="Rockchip">rk</abbr></abbr>ing
	</li>
	<li>
		<strong>Ethernet</strong>
	</li>
	<li>
		<strong>Serial UART</strong> (configured at <strong>115200 bps</strong>, not 1.5Mbps!)
	</li>
	<li>
		<strong>Thermals, CPU and DRAM frequency scaling</strong>
	</li>
	<li>
		<strong>OTG USB 2.0 port </strong>(also as boot device!)
	</li>
	<li>
		<strong>EHCI/OHCI USB 2.0 ports</strong>
	</li>
	<li>
		<strong>MMC subsystem</strong> (including <abbr title="embedded MultiMediaCard"><abbr title="embedded MultiMediaCard">eMMC</abbr></abbr>, SD and sdio devices)
	</li>
	<li>
		<strong>Hardware video acceleration</strong>
	</li>
	<li>
		<strong><abbr title="A type of flash memory"><abbr title="A type of flash memory">NAND</abbr></abbr></strong> is available only on legacy kernel. To fully boot from <abbr title="A type of flash memory"><abbr title="A type of flash memory">NAND</abbr></abbr>, use the <strong>Multitool</strong> and its <strong>steP-<abbr title="A type of flash memory"><abbr title="A type of flash memory">nand</abbr></abbr></strong> installation (instructions are below)
	</li>
	<li>
		<strong>Various WIFI over SDIO are supported (SSV6051</strong><strong>P</strong>, <strong>SSV6256P</strong>, <strong>ESP8089</strong>, <strong>Realtek</strong> chips, etc...), ssv6256p driver is available only on legacy kernel
	</li>
	<li>
		<strong>Full <abbr title="Graphic processing unit (3D acceleration)"><abbr title="Graphic processing unit (3D acceleration)">GPU</abbr></abbr> acceleration</strong>
	</li>
	<li>
		<strong>U-boot</strong> <strong>boot order </strong>priority<strong>:</strong> first the sdcard, then the USB OTG port and eventually the internal <abbr title="embedded MultiMediaCard"><abbr title="embedded MultiMediaCard">eMMC</abbr></abbr>; you can install u-boot (and the whole system) in the internal <abbr title="embedded MultiMediaCard"><abbr title="embedded MultiMediaCard">eMMC</abbr></abbr> and u-boot will always check for images on external sdcard/USB first.
	</li>
</ul>

<p>
	 
</p>

<p>
	<span style="color:#ff00ff;"><span style="font-size:20px;"><strong>Unbrick:</strong></span></span><strong> </strong>
</p>

<p>
	Technically, rockchip devices <strong>cannot be bricked</strong>. If the internal flash does not contain a bootable system, they will always boot from the sdcard. If, for a reason, the bootable system on the internal flash is corrupted or is unable to boot correctly, you can always fo<abbr title="Release candidate"><abbr title="Release candidate">rc</abbr></abbr>e the <strong>maskrom mode</strong> shorting the <abbr title="embedded MultiMediaCard"><abbr title="embedded MultiMediaCard">eMMC</abbr></abbr> clock pin on the PCB. <a href="https://forum.armbian.com/topic/12656-wip-armbian-for-rk322x-devices/?do=findComment&amp;comment=99167" rel="">Here there is the procedure</a>, but you can also google around if you get stuck on a faulty bootloader, the technique is pretty simple and requires a simple screwdriver.
</p>

<p>
	 
</p>

<p>
	There are however some <strong>unfortunate cases</strong> (expecially newer boards) where shorting the <abbr title="embedded MultiMediaCard"><abbr title="embedded MultiMediaCard">eMMC</abbr></abbr> clock pin is difficult or impossibile, like <strong><abbr title="embedded MultiMediaCard"><abbr title="embedded MultiMediaCard">eMMC</abbr></abbr> or eMCP BGA chips</strong> with no exposed pins. In those cases pay double attention when burning something on the internal <abbr title="embedded MultiMediaCard"><abbr title="embedded MultiMediaCard">eMMC</abbr></abbr>/eMCP and <u><strong>always test</strong></u> first the image from the sdcard to be sure it wo<abbr title="Rockchip"><abbr title="Rockchip">rk</abbr></abbr>s before burning anything on <abbr title="embedded MultiMediaCard"><abbr title="embedded MultiMediaCard">eMMC</abbr></abbr>/eMCP.
</p>

<p>
	 
</p>

<p>
	Some useful links with pins, pads or procedures for some boards:
</p>

<ul>
	<li>
		<a href="https://forum.armbian.com/topic/12656-wip-armbian-for-rk322x-devices/?do=findComment&amp;comment=99167" rel="">Generic procedure</a> for boards with non-BGA <abbr title="embedded MultiMediaCard"><abbr title="embedded MultiMediaCard">eMMC</abbr></abbr>
	</li>
	<li>
		<a href="https://forum.armbian.com/topic/12656-csc-armbian-for-rk322x-tv-boxes/?do=findComment&amp;comment=157061" rel="">MXQPRO_V71</a> - eMCP
	</li>
	<li>
		<a href="https://forum.armbian.com/topic/12656-csc-armbian-for-rk322x-tv-boxes/?do=findComment&amp;comment=170092" rel="">H20</a> - eMCP
	</li>
	<li>
		<a href="https://forum.armbian.com/topic/34923-csc-armbian-for-rk322x-tv-box-boards/?do=findComment&amp;comment=183472" rel="">ZQ01</a> - eMCP
	</li>
</ul>

<p>
	 
</p>

<p>
	<span style="color:#ff00ff;"><span style="font-size:20px;"><strong><abbr title="A type of flash memory"><abbr title="A type of flash memory">NAND</abbr></abbr> vs <abbr title="embedded MultiMediaCard"><abbr title="embedded MultiMediaCard">eMMC</abbr></abbr> vs eMCP difference:</strong></span></span>
</p>

<p>
	<abbr title="Rockchip"><abbr title="Rockchip">RK</abbr></abbr>3228 and <abbr title="Rockchip"><abbr title="Rockchip">RK</abbr></abbr>3229 tv boxes comes with three different flash memory chips: <abbr title="embedded MultiMediaCard"><abbr title="embedded MultiMediaCard">eMMC</abbr></abbr>, <abbr title="A type of flash memory"><abbr title="A type of flash memory">NAND</abbr></abbr> and eMCP.
</p>

<p>
	It does not depend upon the ma<abbr title="Rockchip"><abbr title="Rockchip">rk</abbr></abbr>et name of the tv box and neither the internal board; manufacturers put whatever they find cheaper when they buy the components.
</p>

<p>
	 
</p>

<ul>
	<li>
		<abbr title="A type of flash memory"><abbr title="A type of flash memory">NAND</abbr></abbr> chip is <strong>just the non-volatile memory</strong>
	</li>
	<li>
		<abbr title="embedded MultiMediaCard"><abbr title="embedded MultiMediaCard">eMMC</abbr></abbr> chip contains both the non-volatile memory plus a controller.
	</li>
	<li>
		eMCP chip contains the non-volatile memory, a controller for the non-volatile memory (like <abbr title="embedded MultiMediaCard"><abbr title="embedded MultiMediaCard">eMMC</abbr></abbr>), but also contains a bank of DDR SDRAM memory on the same physical chip.
	</li>
</ul>

<p>
	 
</p>

<p>
	The difference is <strong>very</strong> important, because <abbr title="embedded MultiMediaCard"><abbr title="embedded MultiMediaCard">eMMC</abbr></abbr> and eMCP are far easier to support at various levels: the controller deals with the physical characteristics of the non-volatile memory, so the software has no to deal with.
</p>

<p>
	<abbr title="A type of flash memory"><abbr title="A type of flash memory">NAND</abbr></abbr> chips instead are harder to support, because the software is required to deal with the physical characteristics and non-standard things that depends upon the <abbr title="A type of flash memory"><abbr title="A type of flash memory">NAND</abbr></abbr> manufacturer.
</p>

<p>
	 
</p>

<p>
	If you have a <abbr title="A type of flash memory"><abbr title="A type of flash memory">NAND</abbr></abbr> chips you're unlucky because mainline kernel currently cannot access it, but also because you need special care and instructions explained later.
</p>

<p>
	 
</p>

<p>
	You can discover if you have a <abbr title="A type of flash memory"><abbr title="A type of flash memory">NAND</abbr></abbr>, <abbr title="embedded MultiMediaCard"><abbr title="embedded MultiMediaCard">eMMC</abbr></abbr> or eMCP chip looking on the board are reading the signature on the flash memory chip.
</p>

<p>
	The Multitool (see later) also can detect which chip you have onboard: the program will warn you at startup if you have a <abbr title="A type of flash memory"><abbr title="A type of flash memory">NAND</abbr></abbr> chip.
</p>

<p>
	 
</p>

<p>
	<span style="color:#ff00ff;"><span style="font-size:20px;"><strong><abbr title="A type of flash memory"><abbr title="A type of flash memory">NAND</abbr></abbr> bootloader upgrade:</strong></span></span><strong> </strong>
</p>

<p>
	<strong>IMPORTANT:</strong> <span style="color:#e74c3c;"><strong>don't do this is you have an <abbr title="embedded MultiMediaCard"><abbr title="embedded MultiMediaCard">eMMC</abbr> or eMCP</abbr></strong></span>; skip this paragraph if you are unsure too!
</p>

<p>
	For <strong>very expert people</strong> who are having issues when (re)booting images, there is the chance to upgrade the bootloader on <abbr title="A type of flash memory"><abbr title="A type of flash memory">NAND</abbr></abbr>.
</p>

<p>
	The <abbr title="A type of flash memory"><abbr title="A type of flash memory">NAND</abbr></abbr> bootloader is nothing else than a regular <strong>idbloader</strong> (see <a href="http://opensource.rock-chips.com/wiki_Boot_option" rel="external nofollow">official rockchip documentation</a>) but contains some bits to correctly access the data on your flash memory.
</p>

<p>
	Upgrading requires to <strong>erase the existing flash content</strong>, in the worst case will require you to follow the <strong>Unbrick procedure</strong> above or restore an older but more compatible bootloader.
</p>

<p>
	If you are not mentally ready to ove<abbr title="Release candidate"><abbr title="Release candidate">rc</abbr></abbr>ome possible further issues, don't do this!
</p>

<p>
	 
</p>

<p>
	The detailed instructions and the binaries are available <a href="https://forum.armbian.com/topic/12656-csc-armbian-for-rk322x-tv-boxes/?do=findComment&amp;comment=121103" rel="">at this post</a>
</p>

<p>
	 
</p>

<p>
	<span style="color:#ff00ff;"><span style="font-size:20px;"><strong>Multimedia:</strong></span></span>
</p>

<ul>
	<li>
		<strong>Mainline kernel:</strong> <strong>3D acceleration</strong> is provided by Lima driver and is already enabled. <strong>Hardware video decoding</strong>: <a href="https://forum.armbian.com/topic/32449-repository-for-v4l2request-hardware-video-decoding-rockchip-allwinner/" rel="">https://forum.armbian.com/topic/19258-testing-hardware-video-decoding-rockchip-allwinner/</a>
	</li>
	<li>
		<strong>Deprecated legacy kernel:</strong> multimedia features, like <strong>OpenGL/OpenGL ES</strong> acceleration, hardware accelerated <strong>Kodi</strong>, <strong>ffmpeg</strong> and <strong>mpv</strong> you can take a look to<a href="https://forum.armbian.com/topic/12656-wip-armbian-for-rk322x-devices/?do=findComment&amp;comment=102655" rel=""> this post</a>
	</li>
	<li>
		An effective tutorial from <a contenteditable="false" data-ipshover="" data-ipshover-target="https://forum.armbian.com/profile/18531-hai-nguyen/?do=hovercard" data-mentionid="18531" href="https://forum.armbian.com/profile/18531-hai-nguyen/" rel="">@Hai Nguyen</a> <span><span>on how to configure a box as a hi-quality music player using an USB audio card, and controlling it via remote control is available in <a href="https://forum.armbian.com/topic/18178-mxq-rk322x-as-audiophile-music-box/" rel="">this post</a></span></span>
	</li>
</ul>

<p>
	 
</p>

<p>
	<span style="font-size:18px;"><strong><span style="color:#ff00ff;">Brief explanation about kernel naming:</span></strong></span>
</p>

<ul>
	<li>
		<strong>current</strong> kernel is the mainline <abbr title="Long term support"><abbr title="Long term support">LTS</abbr></abbr> kernel version, most maintained and tested. This is the <span style="color:#e74c3c;"><strong>suggested</strong></span> version for production devices. If you don't know what to pick, pick this.
	</li>
	<li>
		<strong>legacy</strong> kernel (version 4.4) is provided by manufacturer; it is<strong> deprecated, unmaintained and not suggested</strong>.
	</li>
	<li>
		<strong>edge</strong> kernel is the development mainline kernel version, with experimental features and drivers; usually stable but perhaps suitable for production devices.
	</li>
</ul>

<p>
	 
</p>

<p>
	You can switch from one kernel flavour to another using <strong>armbian-config </strong>or manually via <strong>apt</strong>.
</p>

<p>
	 
</p>

<p>
	<span style="color:#ff00ff;"><span style="font-size:20px;"><strong>Installation (via SD card):</strong></span></span>
</p>

<p>
	<strong>Building:</strong>
</p>

<p>
	You can build your own image follow the common steps to build armbian for other tv boxes devices: when you are in the moment to choose the target board, switch to <abbr title="Community supported Chip - no official support"><abbr title="Community supported Chip - no official support">CSC</abbr></abbr>/TVB/<abbr title="End of life"><abbr title="End of life">EOL</abbr></abbr> boards and select "<abbr title="Rockchip"><abbr title="Rockchip">rk</abbr></abbr>322x-box" from the list.
</p>

<p>
	 
</p>

<p>
	<span style="color:#e74c3c;"><strong>Download prebuilt images</strong></span> from the following links:
</p>

<ul>
	<li>
		Archive builds (GPG-signed) - <a href="https://imola.armbian.com/dl/rk322x-box/archive/" rel="external nofollow">https://imola.armbian.com/dl/rk322x-box/archive/</a>
	</li>
	<li>
		<strong>SUGGESTED -</strong> Nightly built from trunk each week by Armbian servers (GPG-signed) - <a href="https://github.com/armbian/community" rel="external nofollow">https://github.com/armbian/communit</a>y
	</li>
	<li>
		Old images provided by me (unsigned and outdated) - <a href="https://users.armbian.com/jock/rk322x/armbian/stable" rel="external nofollow">https://users.armbian.com/jock/<abbr title="Rockchip"><abbr title="Rockchip">rk</abbr></abbr>322x/armbian/stable</a>
	</li>
</ul>

<p>
	 
</p>

<p>
	<strong>A<abbr title="Release candidate"><abbr title="Release candidate">rc</abbr></abbr>hived/older images:</strong>
</p>

<p>
	<a href="https://armbian.hosthatch.com/archive/rk322x-box/archive/" rel="external nofollow">https://armbian.hosthatch.com/a<abbr title="Release candidate"><abbr title="Release candidate">rc</abbr></abbr>hive/<abbr title="Rockchip"><abbr title="Rockchip">rk</abbr></abbr>322x-box/a<abbr title="Release candidate"><abbr title="Release candidate">rc</abbr></abbr>hive/</a>
</p>

<p>
	 
</p>

<p>
	<strong>Multitool:</strong>
</p>

<p>
	The Multitool is a small but powerful tool to do quick <strong>backup/restore</strong> of internal flash, but also <strong>burn images</strong> and general <strong>system rescue</strong> and <strong>maintenance</strong> via terminal or <strong>SSH</strong>.
</p>

<p>
	Compressed images will be uncompressed on fly.
</p>

<ul>
	<li>
		<a href="https://users.armbian.com/jock/rk322x/multitool/multitool.img.xz" rel="external nofollow">Multitool - A small but powerful image for <abbr title="Rockchip"><abbr title="Rockchip">RK</abbr></abbr>322x TV Box maintenance</a> (instructions to access via netwo<abbr title="Rockchip"><abbr title="Rockchip">rk</abbr></abbr> <a href="https://forum.armbian.com/topic/12656-csc-armbian-for-rk322x-tv-boxes/?do=findComment&amp;comment=135407" rel="">here</a>)
	</li>
</ul>

<p>
	 
</p>

<p>
	<strong>Quick installation instructions on <abbr title="embedded MultiMediaCard"><abbr title="embedded MultiMediaCard">eMMC</abbr></abbr>:</strong>
</p>

<ul>
	<li>
		Build or download your preferred <strong>Armbian</strong> image and a copy of the <strong>Multitool</strong>;
	</li>
	<li>
		Burn the Multitool on an SD card; once done, place the Armbian image in <strong>images</strong> folder of the SD card NTFS partition;
	</li>
	<li>
		Plug the SD card in the TV box and plug in the power cord. After some seconds the blue led starts blinking and the Multitool appears;
	</li>
	<li>
		<strong><em>OPTIONAL</em></strong>: you can do a backup of the existing firmware with "<strong>Backup flash</strong>" menu option;
	</li>
	<li>
		Choose "<strong>Burn image to flash</strong>" from the menu, then select the destination device (usually <strong>mmcblk2</strong>) and the image to burn;
	</li>
	<li>
		Wait for the process to complete, then choose "<strong>Shutdown"</strong> from main menu;
	</li>
	<li>
		Unplug the power cord and the SD card, then replug the power cord;
	</li>
	<li>
		<strong>Wait for 10 seconds</strong>, then the led should start blinking and HDMI will turn on. The first time the boot process will take a couple of minutes or more because the filesystem is going to be resized, so be patient and wait for the login prompt.
	</li>
	<li>
		On first boot you will be asked for entering a <strong>password for root user</strong> of your choice and <strong>the name and password for a regular user</strong>
	</li>
	<li>
		Run <strong>sudo <abbr title="Rockchip"><abbr title="Rockchip">rk</abbr></abbr>322x-config</strong> and select your board characteristics to enable leds, wifi chips, high-speed <abbr title="embedded MultiMediaCard"><abbr title="embedded MultiMediaCard">eMMC</abbr></abbr>, etc...
	</li>
	<li>
		Run <strong>sudo armbian-config</strong> to configure timezone, locales and other personal options
	</li>
	<li>
		Congratulations, Armbian is now installed and configured!
	</li>
</ul>

<p>
	 
</p>

<p>
	Despite the procedure above is simple and reliable, I always recommend to first test that your device <strong>boots</strong> Armbian images from SD Card.
</p>

<p>
	Due to the really large hardware variety, there is the rare chance that the images proposed here may not boot. If a bad image is burned in <abbr title="embedded MultiMediaCard"><abbr title="embedded MultiMediaCard">eMMC</abbr></abbr>, the box may not boot anymore fo<abbr title="Release candidate"><abbr title="Release candidate">rc</abbr></abbr>ing you to follow the <strong>unbrick</strong> section at the top of this post.
</p>

<p>
	 
</p>

<p>
	<strong>Quick installation instructions on <abbr title="A type of flash memory"><abbr title="A type of flash memory">NAND</abbr></abbr>:</strong>
</p>

<ul>
	<li>
		Build or download your preferred <strong>Armbian</strong> image and a copy of the <strong>Multitool</strong>;
	</li>
	<li>
		Burn the Multitool on an SD card; once done, place the Armbian <span style="color:#e74c3c;"><u><strong>legacy kernel</strong></u></span> image in <strong>images</strong> folder of the SD card NTFS partition;
	</li>
	<li>
		Plug the SD card in the TV box and plug in the power cord. After some seconds the blue led starts blinking and the Multitool appears;
	</li>
	<li>
		<strong><em>OPTIONAL</em></strong>: you can do a backup of the existing firmware with "<strong>Backup flash</strong>" menu option;
	</li>
	<li>
		Choose "<strong>Burn Armbian image via steP-<abbr title="A type of flash memory"><abbr title="A type of flash memory">nand</abbr></abbr></strong>" from the menu, then select the destination device (usually <strong><abbr title="Rockchip"><abbr title="Rockchip">rk</abbr></abbr><abbr title="A type of flash memory"><abbr title="A type of flash memory">nand</abbr></abbr>0</strong>) and the image to burn;
	</li>
	<li>
		Wait for the process to complete, then choose "<strong>Shutdown"</strong> from main menu;
	</li>
	<li>
		Unplug the power cord and the SD card, then replug the power cord;
	</li>
	<li>
		<strong>Wait for 10 seconds</strong>, then the led should start blinking and HDMI will turn on. The first time the boot process will take a couple of minutes or more because the filesystem is going to be resized, so be patient and wait for the login prompt.
	</li>
	<li>
		On first boot you will be asked for entering a <strong>password for root user</strong> of your choice and <strong>the name and password for a regular user</strong>
	</li>
	<li>
		Run <strong>sudo <abbr title="Rockchip"><abbr title="Rockchip">rk</abbr></abbr>322x-config</strong> and select your board characteristics to enable leds, wifi chips, etc...
	</li>
	<li>
		Run <strong>armbian-config</strong> to configure timezone, locales and other personal options
	</li>
	<li>
		Congratulations, Armbian is now installed!
	</li>
</ul>

<p>
	 
</p>

<p>
	<strong>Alternative:</strong> you can install the bootloader in <abbr title="A type of flash memory"><abbr title="A type of flash memory">NAND</abbr></abbr> and let it boot from SD Card or USB:
</p>

<ul>
	<li>
		Download a copy of the <strong><span>Multitool</span></strong> and burn it on an SD card;
	</li>
	<li>
		Plug the SD card in the TV box and plug in the power cord. After some seconds the blue led starts blinking and the Multitool appears;
	</li>
	<li>
		<strong><em>RECOMMENDED: </em></strong>make a backup of the existing firmware with "<strong>Backup flash</strong>" menu option;
	</li>
	<li>
		Choose "<strong>Install Jump Start for Armbian</strong>" menu option: the Jump Start uses the internal <abbr title="A type of flash memory"><abbr title="A type of flash memory">NAND</abbr></abbr> to boot from external SD Card or external USB Stick;
	</li>
	<li>
		Follow the general instructions to boot from SD Card below, <strong>skip the first erase <abbr title="embedded MultiMediaCard"><abbr title="embedded MultiMediaCard">eMMC</abbr></abbr> step</strong>.
	</li>
</ul>

<p>
	 
</p>

<p>
	<strong>Quick installation instructions to boot from SD Card:</strong>
</p>

<ul>
	<li>
		If you are already running Armbian from <abbr title="embedded MultiMediaCard"><abbr title="embedded MultiMediaCard">eMMC</abbr></abbr>, skip to the next step. Instead if you are running the original firmware you need to first erase the internal <abbr title="embedded MultiMediaCard"><abbr title="embedded MultiMediaCard">eMMC</abbr></abbr>; to do so download the <strong>Multitool</strong>, burn it on an SD Card, plug the SD Card and power the TV Box. Use "<strong>Backup flash</strong>" if you want to do a backup of the existing firmware, then choose "<strong>Erase flash</strong>" menu option.
	</li>
	<li>
		Build or download your preferred <strong>Armbian</strong> image;
	</li>
	<li>
		Uncompress and burn the Armbian image on the SD Card;
	</li>
	<li>
		Plug the SD Card in the TV Box and power it on;
	</li>
	<li>
		<strong>Wait for 10 seconds</strong>, then the led should start blinking and HDMI will turn on. The first time the boot process will take a couple of minutes or more because the filesystem is going to be resized, so be patient and wait for the login prompt;
	</li>
	<li>
		On first boot you will be asked for entering a <strong>password for root user</strong> of your choice and <strong>the name and password for a regular user</strong>
	</li>
	<li>
		Run <strong>sudo <abbr title="Rockchip"><abbr title="Rockchip">rk</abbr></abbr>322x-config</strong> and select your board characteristics to enable leds, wifi chips, high-speed <abbr title="embedded MultiMediaCard"><abbr title="embedded MultiMediaCard">eMMC</abbr></abbr> or <abbr title="A type of flash memory"><abbr title="A type of flash memory">NAND</abbr></abbr>, etc...
	</li>
	<li>
		Run <strong>armbian-config</strong> to configure timezone, locales and other personal options, or also to transfer the SD Card installation to internal <abbr title="embedded MultiMediaCard"><abbr title="embedded MultiMediaCard">eMMC</abbr></abbr>;
	</li>
	<li>
		Congratulations, Armbian is running from SD Card!
	</li>
</ul>

<p>
	 
</p>

<p>
	<strong>A note about boot device order:</strong>
</p>

<p>
	With Armbian also comes mainline U-boot. If you install Armbian or just the bootloader in the <abbr title="embedded MultiMediaCard"><abbr title="embedded MultiMediaCard">eMMC</abbr></abbr> or the Jump Start on internal <abbr title="A type of flash memory"><abbr title="A type of flash memory">NAND</abbr></abbr>, the bootloader will look for valid bootable images in this order:
</p>

<ul>
	<li>
		<strong>External SD Card</strong>
	</li>
	<li>
		<strong>External USB Stick </strong>in OTG Port
	</li>
	<li>
		<strong>Internal</strong> <strong><abbr title="embedded MultiMediaCard"><abbr title="embedded MultiMediaCard">eMMC</abbr></abbr></strong>
	</li>
</ul>

<p>
	 
</p>

<p>
	<strong><span style="font-size:20px;"><span style="color:#ff00ff;">Installation (without SD card, board with <abbr title="embedded MultiMediaCard"><abbr title="embedded MultiMediaCard">eMMC</abbr>)</abbr></span></span></strong>
</p>

<p>
	If you have no sd card slot and your board has an <abbr title="embedded MultiMediaCard"><abbr title="embedded MultiMediaCard">eMMC</abbr></abbr>, you can burn the armbian image directly on the internal <abbr title="embedded MultiMediaCard"><abbr title="embedded MultiMediaCard">eMMC</abbr></abbr> using <abbr title="Rockchip"><abbr title="Rockchip">rk</abbr></abbr>developtool and a male-to-male USB cable:
</p>

<p>
	 
</p>

<ul>
	<li>
		Download your preferred Armbian image from<a href="https://www.armbian.com/download/" rel="external nofollow"> Armbian download page</a> and <strong>decompress it</strong>.
	</li>
	<li>
		Download the <abbr title="Rockchip"><abbr title="Rockchip">rk</abbr></abbr>322x bootloader: <a class="ipsAttachLink" data-fileext="bin" data-fileid="10453" href="https://forum.armbian.com/applications/core/interface/file/attachment.php?id=10453&amp;key=291f13cf8ce42009eb4ef3266f27a325" rel="">rk322x_loader_v1.10.238_256.bin</a>
	</li>
	<li>
		Download a copy of <strong><abbr title="Rockchip"><abbr title="Rockchip">rk</abbr></abbr>developtool: </strong>a compiled binary is<strong> </strong>available in the official <a href="https://github.com/rockchip-linux/rkbin" rel="external nofollow">rockchip-linux <abbr title="Rockchip"><abbr title="Rockchip">rk</abbr></abbr>bin github repository</a>.
	</li>
	<li>
		<strong>Unplug</strong> the power cord from the tv box
	</li>
	<li>
		Plug an end of an USB Male-to-male cable into the OTG port (normally it is the lone USB port on the same side of the Ethernet, HDMI, analog AV connectors) <strong>while pressing the</strong> <strong>reset microbutton </strong>with a toothpick. You can find the reset microbutton in a hole in the back of the box, but sometimes it is hidden into the AV analog jack
	</li>
	<li>
		Plug the other end of the USB Male-to-male cable into an USB port of your computer
	</li>
	<li>
		If everything went well, run <strong>lsusb</strong>: you should see a device with <strong>ID</strong> <strong>2207:320b</strong>
	</li>
	<li>
		Run <strong>sudo <abbr title="Rockchip"><abbr title="Rockchip">rk</abbr></abbr>developtool rd 3</strong> (if this fails don't worry and <strong>proceed to next step</strong>)
	</li>
	<li>
		Run <strong>sudo <abbr title="Rockchip"><abbr title="Rockchip">rk</abbr></abbr>developtool db <abbr title="Rockchip"><abbr title="Rockchip">rk</abbr></abbr>322x_loader_v1.10.238_256.bin</strong>
	</li>
	<li>
		Run <strong>sudo <abbr title="Rockchip"><abbr title="Rockchip">rk</abbr></abbr>developtool wl 0x0 image.img</strong> (change <strong>image.img</strong> this with the real Armbian image filename)
	</li>
	<li>
		Unplug the power cord
	</li>
	<li>
		Done!
	</li>
</ul>

<p>
	 
</p>

<p>
	<strong><span style="font-size:20px;"><span style="color:#ff00ff;">Installation (without SD card, board with <abbr title="A type of flash memory"><abbr title="A type of flash memory">NAND</abbr></abbr>)</span></span></strong>
</p>

<p>
	If you are in the unfortunate case you can't use an SD card for installation and your board has a <abbr title="A type of flash memory"><abbr title="A type of flash memory">NAND</abbr></abbr> chip, you still have an option to use the quick Multitool installation steps via USB.
</p>

<p>
	 
</p>

<ul>
	<li>
		Obtain a copy of <strong><abbr title="Rockchip"><abbr title="Rockchip">rk</abbr></abbr>developtool: </strong>a compiled binary is<strong> </strong>available in the official <a href="https://github.com/rockchip-linux/rkbin" rel="external nofollow">rockchip-linux <abbr title="Rockchip"><abbr title="Rockchip">rk</abbr></abbr>bin github repository</a>.
	</li>
	<li>
		<strong>Unplug</strong> the power cord from the tv box
	</li>
	<li>
		Plug an end of an USB Male-to-male cable into the OTG port (normally it is the lone USB port on the same side of the Ethernet, HDMI, analog AV connectors) <strong>while pressing the</strong> <strong>reset microbutton </strong>with a toothpick. You can find the reset microbutton in a hole in the back of the box, but sometimes it is hidden into the AV analog jack
	</li>
	<li>
		Plug the other end of the USB Male-to-male cable into an USB port of your computer
	</li>
	<li>
		If everyting went well, using <strong>lsusb</strong> you should see a device with <strong>ID</strong> <strong>2207:320b</strong>
	</li>
	<li>
		Run <strong>sudo <abbr title="Rockchip"><abbr title="Rockchip">rk</abbr></abbr>developtool wl 0x4000 u-boot-main.img</strong> (download <a class="ipsAttachLink" contenteditable="false" data-fileext="xz" data-fileid="7647" href="https://forum.armbian.com/applications/core/interface/file/attachment.php?id=7647" rel="">u-boot-main.img.xz</a> , don't forget to decompress it!)
	</li>
	<li>
		Unplug the power cord
	</li>
</ul>

<p>
	 
</p>

<p>
	Now you can follow the instructions on how to install on <abbr title="embedded MultiMediaCard"><abbr title="embedded MultiMediaCard">eMMC</abbr></abbr>/<abbr title="A type of flash memory"><abbr title="A type of flash memory">NAND</abbr></abbr> via SD card, just use instead an USB stick to do all the operations and plug it into the USB OTG port. Once you reboot, USB OTG port will be used as a boot device.
</p>

<p>
	 
</p>

<p>
	<strong><span style="color:#e74c3c;">NOTE: </span></strong><abbr title="A type of flash memory"><abbr title="A type of flash memory">NAND</abbr></abbr> users without SD slot may be unhappy to know that it will be difficult to do extra maintenance with Multitool in case something breaks in the installed Armbian system: installing <strong>u-boot-main.img </strong>makes the installed system unbootable because it is missing the <abbr title="A type of flash memory"><abbr title="A type of flash memory">NAND</abbr></abbr> driver.
</p>

<p>
	 
</p>

<p>
	 
</p>

<p>
	<strong>Alternative backup, restore and erase flash <span style="color:#e74c3c;">for EXPERTS</span>:</strong>
</p>

<p>
	These backup, restore and erase flash procedures are for experts only. They are kept here mostly for reference, since the <strong>Multitool</strong> is perfectly able to do same from a very comfy interface and is the suggested way to do maintenance.
</p>

<p>
	 
</p>

<p>
	<strong>Backup</strong>:
</p>

<ul>
	<li>
		Obtain a copy of <strong><abbr title="Rockchip"><abbr title="Rockchip">rk</abbr></abbr>developtool: </strong>a compiled binary is<strong> </strong>available in the official <a href="https://github.com/rockchip-linux/rkbin" rel="external nofollow">rockchip-linux <abbr title="Rockchip"><abbr title="Rockchip">rk</abbr></abbr>bin github repository</a>. If you prefer, you can compile it yourself from the sou<abbr title="Release candidate"><abbr title="Release candidate">rc</abbr></abbr>es available at <a href="https://github.com/rockchip-linux/rkdeveloptool" rel="external nofollow">official rockchip repository</a>
	</li>
	<li>
		<strong>Unplug</strong> the power cord from the tv box
	</li>
	<li>
		Plug an end of an USB Male-to-male cable into the OTG port (normally it is the lone USB port on the same side of the Ethernet, HDMI, analog AV connectors) <strong>while pressing the reset microbutton with a toothpick</strong>. You can find the reset microbutton in a hole in the back of the box, but sometimes it is hidden into the AV analog jack
	</li>
	<li>
		Plug the other end of the USB Male-to-male cable into an USB port of your computer
	</li>
	<li>
		If everyting went well, using <strong>lsusb</strong> you should see a device with <strong>ID</strong> <strong>2207:320b</strong>
	</li>
	<li>
		change directory and move into <strong><abbr title="Rockchip"><abbr title="Rockchip">rk</abbr></abbr>bin/tools </strong>directory, run <strong>./<abbr title="Rockchip"><abbr title="Rockchip">rk</abbr></abbr>developtool rfi</strong> then take note of the <strong>FLASH SIZE</strong> megabytes (my <abbr title="embedded MultiMediaCard"><abbr title="embedded MultiMediaCard">eMMC</abbr></abbr> is 8Gb, <abbr title="Rockchip"><abbr title="Rockchip">rk</abbr></abbr>developtool reports <strong>7393 megabytes</strong>)
	</li>
	<li>
		run <strong>./<abbr title="Rockchip"><abbr title="Rockchip">rk</abbr></abbr>developtool rl 0x0 $((FLASH_SIZE * 2048)) backup.data</strong> (change FLASH_SIZE with the value you obtained the step before)
	</li>
	<li>
		once done, the internal <abbr title="embedded MultiMediaCard"><abbr title="embedded MultiMediaCard">eMMC</abbr></abbr> is backed up to <strong>backup.data</strong> file
	</li>
</ul>

<p>
	 
</p>

<p>
	<strong>Restore</strong>: first we have to restore the original bootloader, then restore the original firmware.<br />
	Running <strong><abbr title="Rockchip"><abbr title="Rockchip">rk</abbr></abbr>developtool </strong>with these switches will accomplish both the jobs:
</p>

<pre class="ipsCode">./rkdeveloptool db rk322x_loader_v1.10.238_256.bin
Downloading bootloader succeeded.

./rkdeveloptool ul rk322x_loader_v1.10.238_256.bin
Upgrading loader succeeded.

./rkdeveloptool wl 0x0 backup.data
Write LBA from file (100%)</pre>

<p>
	Download here:
</p>

<p>
	 
</p>

<p>
	<strong>Erase the <abbr title="embedded MultiMediaCard">flash memory</abbr>: </strong>clearing the internal <abbr title="embedded MultiMediaCard"><abbr title="embedded MultiMediaCard">eMMC</abbr></abbr>/<abbr title="A type of flash memory"><abbr title="A type of flash memory">NAND</abbr></abbr> memory makes the <abbr title="System On a Chip"><abbr title="System On a Chip">SoC</abbr></abbr> look for external SD Card as first boot option.
</p>

<p>
	If there isn't any suitable SD Card, the <abbr title="System On a Chip"><abbr title="System On a Chip">SoC</abbr></abbr> enters <strong>maskrom mode</strong>, which can then be used for full <abbr title="embedded MultiMediaCard"><abbr title="embedded MultiMediaCard">eMMC</abbr></abbr>/<abbr title="A type of flash memory"><abbr title="A type of flash memory">NAND</abbr></abbr> access using <abbr title="Rockchip"><abbr title="Rockchip">rk</abbr></abbr>developtool. This is perfectly fine if your box has an <abbr title="embedded MultiMediaCard"><abbr title="embedded MultiMediaCard">eMMC</abbr></abbr> flash memory.
</p>

<p>
	<span style="color:#e74c3c;"><strong>NOTE:</strong></span> In case you have a <abbr title="A type of flash memory"><abbr title="A type of flash memory">NAND</abbr></abbr> flash memory this option is however <strong>discouraged</strong>. The original bootloader contains some special parameters to correctly access the data. Clearing the flash memory will probably garbage the <abbr title="A type of flash memory"><abbr title="A type of flash memory">NAND</abbr></abbr> data and restoring the bootloader may require some special instructions.
</p>

<p>
	 
</p>

<ul>
	<li>
		Obtain a copy of <strong><abbr title="Rockchip"><abbr title="Rockchip">rk</abbr></abbr>developtool: </strong>a compiled binary is<strong> </strong>available in the official <a href="https://github.com/rockchip-linux/rkbin" rel="external nofollow">rockchip-linux <abbr title="Rockchip"><abbr title="Rockchip">rk</abbr></abbr>bin github repository</a>. If you prefer, you can compile it yourself from the sou<abbr title="Release candidate"><abbr title="Release candidate">rc</abbr></abbr>es available at <a href="https://github.com/rockchip-linux/rkdeveloptool" rel="external nofollow">official rockchip repository</a>
	</li>
	<li>
		<strong>Unplug</strong> the power cord from the board
	</li>
	<li>
		Plug an end of an USB Male-to-male cable into the OTG port (normally it is the lone USB port on the same side of the Ethernet, HDMI, analog AV connectors) <strong>while pressing the reset microbutton with a toothpick</strong>. You can find the reset microbutton in a hole in the back of the box, but sometimes it is hidden into the AV analog jack
	</li>
	<li>
		Plug the other end of the USB Male-to-male cable into an USB port of your computer
	</li>
	<li>
		If everyting went well, using <strong>lsusb</strong> you should see a device with <strong>ID</strong> <strong>2207:320b</strong>
	</li>
	<li>
		run <strong>./<abbr title="Rockchip"><abbr title="Rockchip">rk</abbr></abbr>developtool ef</strong> and wait a few seconds
	</li>
	<li>
		once done, the internal <abbr title="embedded MultiMediaCard"><abbr title="embedded MultiMediaCard">eMMC</abbr></abbr> is erased and the device will boot from the sdcard from now on
	</li>
</ul>

<p>
	 
</p>

<p>
	<span style="font-size:24px;"><strong><span style="color:#ff00ff;">Partecipation and debugging:</span></strong></span>
</p>

<p>
	If you want to partecipate or need help debugging issues, do not hesitate to share your experience with the installation procedure of the boxes.
</p>

<p>
	In case of issues and missed support, provide as many as possible of these things is very useful to try and bring support for an unsupported board:
</p>

<p>
	 
</p>

<ul>
	<li>
		some photos of both sides of the board. Details of the <abbr title="embedded MultiMediaCard"><abbr title="embedded MultiMediaCard">eMMC</abbr></abbr>, DDR and Wifi chips are very useful!
	</li>
	<li>
		upload the device tree binary (<abbr title="Device tree blob"><abbr title="Device tree blob">dtb</abbr></abbr>) of your device. We can understand a lot of things of the hardware from that small piece of data; and alternative is a link to the original firmware (you can do a full backup with the Multitool);
	</li>
	<li>
		dmesg and other logs (use armbianmonitor -u that automatically collects and uploads the logs online)
	</li>
	<li>
		attach a serial converter to the device and provide the output of the serial port;
	</li>
</ul>

<p>
	 
</p>

<p>
	Critics, suggestions and contributions are welcome!
</p>

<p>
	 
</p>

<p>
	<strong>Credits:</strong>
</p>

<ul>
	<li>
		<strong><a contenteditable="false" data-ipshover="" data-ipshover-target="https://forum.armbian.com/profile/13603-fabiobassa/?do=hovercard" data-mentionid="13603" href="https://forum.armbian.com/profile/13603-fabiobassa/" rel="">@fabiobassa</a> </strong>for his ideas, inspiration, great generosity in giving the boards for development and testing. The project of bringing <abbr title="Rockchip"><abbr title="Rockchip">rk</abbr></abbr>322x into armbian would not have begun without his support!
	</li>
	<li>
		Justin Swartz, for his wo<abbr title="Rockchip"><abbr title="Rockchip">rk</abbr></abbr> and resea<abbr title="Release candidate"><abbr title="Release candidate">rc</abbr></abbr>h to bring mainline linux on <abbr title="Rockchip"><abbr title="Rockchip">rk</abbr></abbr>3229 (repository <a href="https://github.com/jhswartz/rk3229" rel="external nofollow">here</a>)
	</li>
	<li>
		<span><a contenteditable="false" data-ipshover="" data-ipshover-target="https://forum.armbian.com/profile/14286-knaerzche/?do=hovercard" data-mentionid="14286" href="https://forum.armbian.com/profile/14286-knaerzche/" rel="">@knaerzche</a> for his great contribution to libreelec support and mainline patches</span>
	</li>
	<li>
		<a contenteditable="false" data-ipshover="" data-ipshover-target="https://forum.armbian.com/profile/8117-alex83/?do=hovercard" data-mentionid="8117" href="https://forum.armbian.com/profile/8117-alex83/" rel="">@Alex83</a> for his patience in testing the <abbr title="A type of flash memory"><abbr title="A type of flash memory">NAND</abbr></abbr> bootloader upgrade procedure on his board
	</li>
	<li>
		<a contenteditable="false" data-ipshover="" data-ipshover-target="https://forum.armbian.com/profile/34993-jason-duhamell/?do=hovercard" data-mentionid="34993" href="https://forum.armbian.com/profile/34993-jason-duhamell/" rel="">@Jason Duhamell</a> for his generous donation that allowed resea<abbr title="Release candidate"><abbr title="Release candidate">rc</abbr></abbr>hing eMCP boards and esp8089 wifi chip
	</li>
</ul>


			
		</div>

		
			<div class="ipsItemControls">
				
					
						

	<div data-controller='core.front.core.reaction' class='ipsItemControls_right ipsClearfix '>	
		<div class='ipsReact ipsPos_right'>
			
				
				<div class='ipsReact_reactCountOnly ipsAreaBackground_positive ipsType_blendLinks ' data-role='reactCount'>
					
						<a href='https://forum.armbian.com/topic/34923-csc-armbian-for-rk322x-tv-box-boards/?do=showReactionsComment&amp;comment=92569' data-ipsTooltip title='See who reacted to this' data-ipsDialog data-ipsDialog-title='See who reacted to this'>
					
					<span data-role='reactCountText'>20</span>
					
						</a>
					
				</div>
			
			
			
		</div>
	</div>

					
				
				<ul class="ipsComment_controls ipsClearfix ipsItemControls_left" data-role="commentControls">
					
						
						
							<li data-ipsquote-editor="topic_comment" data-ipsquote-target="#comment-92569" class="ipsJS_show ipsHide">
								<button class="ipsButton ipsButton_light ipsButton_verySmall ipsButton_narrow cMultiQuote ipsHide" data-action="multiQuoteComment" data-ipstooltip data-ipsquote-multiquote data-mqid="mq92569" title="MultiQuote"><i class="fa fa-plus"></i></button>
							</li>
							<li data-ipsquote-editor="topic_comment" data-ipsquote-target="#comment-92569" class="ipsJS_show ipsHide">
								<a href="#" data-action="quoteComment" data-ipsquote-singlequote>Quote</a>
							</li>
						
						
						
												
					
					<li class="ipsHide" data-role="commentLoading">
						<span class="ipsLoading ipsLoading_tiny ipsLoading_noAnim"></span>
					</li>
				</ul>
			</div>
		

		
	</div>

	

	



<div class='ipsPadding ipsHide cPostShareMenu' id='elShareComment_92569_menu'>
	<h5 class='ipsType_normal ipsType_reset'>Link to comment</h5>
	
		
	
	
	<input type='text' value='https://forum.armbian.com/topic/34923-csc-armbian-for-rk322x-tv-box-boards/' class='ipsField_fullWidth'>

	
	<h5 class='ipsType_normal ipsType_reset ipsSpacer_top'>Share on other sites</h5>
	

	<ul class='ipsList_inline ipsList_noSpacing ipsClearfix' data-controller="core.front.core.sharelink">
		
			<li>
<a href="https://www.linkedin.com/shareArticle?mini=true&amp;url=https%3A%2F%2Fforum.armbian.com%2Ftopic%2F34923-csc-armbian-for-rk322x-tv-box-boards%2F%3Fdo%3DfindComment%26comment%3D92569&amp;title=CSC+Armbian+for+RK322x+TV+box+boards" rel="nofollow noopener" class="cShareLink cShareLink_linkedin" target="_blank" data-role="shareLink" title='Share on LinkedIn' data-ipsTooltip>
	<i class="fa fa-linkedin"></i>
</a></li>
		
			<li>
<a href="https://x.com/share?url=https%3A%2F%2Fforum.armbian.com%2Ftopic%2F34923-csc-armbian-for-rk322x-tv-box-boards%2F%3Fdo%3DfindComment%26comment%3D92569" class="cShareLink cShareLink_x" target="_blank" data-role="shareLink" title='Share on X' data-ipsTooltip rel='nofollow noopener'>
    <i class="fa fa-x"></i>
</a></li>
		
			<li>
<a href="https://www.facebook.com/sharer/sharer.php?u=https%3A%2F%2Fforum.armbian.com%2Ftopic%2F34923-csc-armbian-for-rk322x-tv-box-boards%2F%3Fdo%3DfindComment%26comment%3D92569" class="cShareLink cShareLink_facebook" target="_blank" data-role="shareLink" title='Share on Facebook' data-ipsTooltip rel='noopener nofollow'>
	<i class="fa fa-facebook"></i>
</a></li>
		
			<li>
<a href="https://www.reddit.com/submit?url=https%3A%2F%2Fforum.armbian.com%2Ftopic%2F34923-csc-armbian-for-rk322x-tv-box-boards%2F%3Fdo%3DfindComment%26comment%3D92569&amp;title=CSC+Armbian+for+RK322x+TV+box+boards" rel="nofollow noopener" class="cShareLink cShareLink_reddit" target="_blank" title='Share on Reddit' data-ipsTooltip>
	<i class="fa fa-reddit"></i>
</a></li>
		
			<li>
<a href="mailto:?subject=CSC%20Armbian%20for%20RK322x%20TV%20box%20boards&body=https%3A%2F%2Fforum.armbian.com%2Ftopic%2F34923-csc-armbian-for-rk322x-tv-box-boards%2F%3Fdo%3DfindComment%26comment%3D92569" rel='nofollow' class='cShareLink cShareLink_email' title='Share via email' data-ipsTooltip>
	<i class="fa fa-envelope"></i>
</a></li>
		
	</ul>


	<hr class='ipsHr'>
	<button class='ipsHide ipsButton ipsButton_small ipsButton_light ipsButton_fullWidth ipsMargin_top:half' data-controller='core.front.core.webshare' data-role='webShare' data-webShareTitle='CSC Armbian for RK322x TV box boards' data-webShareText='DISCLAIMERS (PLEASE READ):
 


	
		Everything you can find in this thread (binaries, texts, code snippets, etc...) are provided AS-IS and are not part of official Armbian project. For this reason not people from Armbian project nor myself are responsible for misuse or loss of functionality of hardware.
	 
	
		THIS POST explains very well the troubles with TV Boxes and why they are not suitable for everyone
	 
	
		Please don&#039;t ask about support or assistance in other non-community forums nor in the official Armbian github repository, instead post your questions in this thread, in the TV Boxes forum section (hardware related) or in the Peer-to-peer support section (general linux/software related).
	 



	 
 


	Following the recent thread on LibreElec forum about an unofficial image for rk3229 devices, I would like to make public the work made by me and @fabiobassa about bringing rk322x support to armbian.
 


	The project is now in -&gt; mainline Armbian &lt;- development fork -&gt; here &lt;-
 


	 
 


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
 


	 
 


	If you have a NAND chips you&#039;re unlucky because mainline kernel currently cannot access it, but also because you need special care and instructions explained later.
 


	 
 


	You can discover if you have a NAND, eMMC or eMCP chip looking on the board are reading the signature on the flash memory chip.
 


	The Multitool (see later) also can detect which chip you have onboard: the program will warn you at startup if you have a NAND chip.
 


	 
 


	NAND bootloader upgrade: 
 


	IMPORTANT: don&#039;t do this is you have an eMMC or eMCP; skip this paragraph if you are unsure too!
 


	For very expert people who are having issues when (re)booting images, there is the chance to upgrade the bootloader on NAND.
 


	The NAND bootloader is nothing else than a regular idbloader (see official rockchip documentation) but contains some bits to correctly access the data on your flash memory.
 


	Upgrading requires to erase the existing flash content, in the worst case will require you to follow the Unbrick procedure above or restore an older but more compatible bootloader.
 


	If you are not mentally ready to overcome possible further issues, don&#039;t do this!
 


	 
 


	The detailed instructions and the binaries are available at this post
 


	 
 


	Multimedia:
 


	
		Mainline kernel: 3D acceleration is provided by Lima driver and is already enabled. Hardware video decoding: https://forum.armbian.com/topic/19258-testing-hardware-video-decoding-rockchip-allwinner/
	 
	
		Deprecated legacy kernel: multimedia features, like OpenGL/OpenGL ES acceleration, hardware accelerated Kodi, ffmpeg and mpv you can take a look to this post
	 
	
		An effective tutorial from @Hai Nguyen on how to configure a box as a hi-quality music player using an USB audio card, and controlling it via remote control is available in this post
	 



	 
 


	Brief explanation about kernel naming:
 


	
		current kernel is the mainline LTS kernel version, most maintained and tested. This is the suggested version for production devices. If you don&#039;t know what to pick, pick this.
	 
	
		legacy kernel (version 4.4) is provided by manufacturer; it is deprecated, unmaintained and not suggested.
	 
	
		edge kernel is the development mainline kernel version, with experimental features and drivers; usually stable but perhaps suitable for production devices.
	 



	 
 


	You can switch from one kernel flavour to another using armbian-config or manually via apt.
 


	 
 


	Installation (via SD card):
 


	Building:
 


	You can build your own image follow the common steps to build armbian for other tv boxes devices: when you are in the moment to choose the target board, switch to CSC/TVB/EOL boards and select &quot;rk322x-box&quot; from the list.
 


	 
 


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
	 
	
		OPTIONAL: you can do a backup of the existing firmware with &quot;Backup flash&quot; menu option;
	 
	
		Choose &quot;Burn image to flash&quot; from the menu, then select the destination device (usually mmcblk2) and the image to burn;
	 
	
		Wait for the process to complete, then choose &quot;Shutdown&quot; from main menu;
	 
	
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
	 
	
		OPTIONAL: you can do a backup of the existing firmware with &quot;Backup flash&quot; menu option;
	 
	
		Choose &quot;Burn Armbian image via steP-nand&quot; from the menu, then select the destination device (usually rknand0) and the image to burn;
	 
	
		Wait for the process to complete, then choose &quot;Shutdown&quot; from main menu;
	 
	
		Unplug the power cord and the SD card, then replug the power cord;
	 
	
		Wait for 10 seconds, then the led should start blinking and HDMI will turn on. The first time the boot process will take a couple of minutes or more because the filesystem is going to be resized, so be patient and wait for the login prompt.
	 
	
		On first boot you will be asked for entering a password for root user of your choice and the name and password for a regular user
	 
	
		Run sudo rk322x-config and select your board characteristics to enable leds, wifi chips, etc...
	 
	
		Run armbian-config to configure timezone, locales and other personal options
	 
	
		Congratulations, Armbian is now installed!
	 



	 
 


	Alternative: you can install the bootloader in NAND and let it boot from SD Card or USB:
 


	
		Download a copy of the Multitool and burn it on an SD card;
	 
	
		Plug the SD card in the TV box and plug in the power cord. After some seconds the blue led starts blinking and the Multitool appears;
	 
	
		RECOMMENDED: make a backup of the existing firmware with &quot;Backup flash&quot; menu option;
	 
	
		Choose &quot;Install Jump Start for Armbian&quot; menu option: the Jump Start uses the internal NAND to boot from external SD Card or external USB Stick;
	 
	
		Follow the general instructions to boot from SD Card below, skip the first erase eMMC step.
	 



	 
 


	Quick installation instructions to boot from SD Card:
 


	
		If you are already running Armbian from eMMC, skip to the next step. Instead if you are running the original firmware you need to first erase the internal eMMC; to do so download the Multitool, burn it on an SD Card, plug the SD Card and power the TV Box. Use &quot;Backup flash&quot; if you want to do a backup of the existing firmware, then choose &quot;Erase flash&quot; menu option.
	 
	
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
	 
	
		Run sudo rkdeveloptool rd 3 (if this fails don&#039;t worry and proceed to next step)
	 
	
		Run sudo rkdeveloptool db rk322x_loader_v1.10.238_256.bin
	 
	
		Run sudo rkdeveloptool wl 0x0 image.img (change image.img this with the real Armbian image filename)
	 
	
		Unplug the power cord
	 
	
		Done!
	 



	 
 


	Installation (without SD card, board with NAND)
 


	If you are in the unfortunate case you can&#039;t use an SD card for installation and your board has a NAND chip, you still have an option to use the quick Multitool installation steps via USB.
 


	 
 


	
		Obtain a copy of rkdeveloptool: a compiled binary is available in the official rockchip-linux rkbin github repository.
	 
	
		Unplug the power cord from the tv box
	 
	
		Plug an end of an USB Male-to-male cable into the OTG port (normally it is the lone USB port on the same side of the Ethernet, HDMI, analog AV connectors) while pressing the reset microbutton with a toothpick. You can find the reset microbutton in a hole in the back of the box, but sometimes it is hidden into the AV analog jack
	 
	
		Plug the other end of the USB Male-to-male cable into an USB port of your computer
	 
	
		If everyting went well, using lsusb you should see a device with ID 2207:320b
	 
	
		Run sudo rkdeveloptool wl 0x4000 u-boot-main.img (download u-boot-main.img.xz , don&#039;t forget to decompress it!)
	 
	
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
 


	If there isn&#039;t any suitable SD Card, the SoC enters maskrom mode, which can then be used for full eMMC/NAND access using rkdeveloptool. This is perfectly fine if your box has an eMMC flash memory.
 


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
	 

' data-webShareUrl='https://forum.armbian.com/topic/34923-csc-armbian-for-rk322x-tv-box-boards/?do=findComment&amp;comment=92569'>More sharing options...</button>

	
</div>
</div>

	</div>
</article>

					
					
					
				

					

					
					



<a id="comment-92583"></a>
<article  id="elComment_92583" class="cPost ipsBox ipsResponsive_pull  ipsComment  ipsComment_parent ipsClearfix ipsClear ipsColumns ipsColumns_noSpacing ipsColumns_collapsePhone    ">
	

	

	<div class="cAuthorPane_mobile ipsResponsive_showPhone">
		<div class="cAuthorPane_photo">
			<div class="cAuthorPane_photoWrap">
				


	<a href="https://forum.armbian.com/profile/13603-fabiobassa/" rel="nofollow" data-ipsHover data-ipsHover-width="370" data-ipsHover-target="https://forum.armbian.com/profile/13603-fabiobassa/?do=hovercard" class="ipsUserPhoto ipsUserPhoto_large" title="Go to fabiobassa's profile">
		<img src='https://forum.armbian.com/uploads/monthly_2020_06/download.thumb.jpg.c76c16404011b211742cd18e0cae7edc.jpg' alt='fabiobassa' loading="lazy">
	</a>

				
				
			</div>
		</div>
		<div class="cAuthorPane_content">
			<h3 class="ipsType_sectionHead cAuthorPane_author ipsType_break ipsType_blendLinks ipsFlex ipsFlex-ai:center">
				



<a href="https://forum.armbian.com/profile/13603-fabiobassa/" rel="nofollow" data-ipshover data-ipshover-width="370" data-ipshover-target="https://forum.armbian.com/profile/13603-fabiobassa/?do=hovercard&amp;referrer=https%253A%252F%252Fforum.armbian.com%252Ftopic%252F34923-csc-armbian-for-rk322x-tv-box-boards%252F" title="Go to fabiobassa's profile" class="ipsType_break">fabiobassa</a>
			</h3>
			<div class="ipsType_light ipsType_reset">
				<a href="https://forum.armbian.com/topic/34923-csc-armbian-for-rk322x-tv-box-boards/?do=findComment&amp;comment=92583" rel="nofollow" class="ipsType_blendLinks">Posted <time datetime='2020-01-08T22:09:53Z' title='01/08/20 10:09  PM' data-short='4 yr'>January 8, 2020</time></a>
				
			</div>
		</div>
	</div>
	<aside class="ipsComment_author cAuthorPane ipsColumn ipsColumn_medium ipsResponsive_hidePhone">
		<h3 class="ipsType_sectionHead cAuthorPane_author ipsType_blendLinks ipsType_break">
<strong>



<a href="https://forum.armbian.com/profile/13603-fabiobassa/" rel="nofollow" data-ipshover data-ipshover-width="370" data-ipshover-target="https://forum.armbian.com/profile/13603-fabiobassa/?do=hovercard&amp;referrer=https%253A%252F%252Fforum.armbian.com%252Ftopic%252F34923-csc-armbian-for-rk322x-tv-box-boards%252F" title="Go to fabiobassa's profile" class="ipsType_break">fabiobassa</a></strong>
			
		</h3>
		<ul class="cAuthorPane_info ipsList_reset">
			<li data-role="photo" class="cAuthorPane_photo">
				<div class="cAuthorPane_photoWrap">
					


	<a href="https://forum.armbian.com/profile/13603-fabiobassa/" rel="nofollow" data-ipsHover data-ipsHover-width="370" data-ipsHover-target="https://forum.armbian.com/profile/13603-fabiobassa/?do=hovercard" class="ipsUserPhoto ipsUserPhoto_large" title="Go to fabiobassa's profile">
		<img src='https://forum.armbian.com/uploads/monthly_2020_06/download.thumb.jpg.c76c16404011b211742cd18e0cae7edc.jpg' alt='fabiobassa' loading="lazy">
	</a>

					
					
				</div>
			</li>
			
				

	<li data-role="group">
		Members
	</li>







	
	
	
		
	
	

				
			
			
				<li data-role="stats" class="ipsMargin_top">
					<ul class="ipsList_reset ipsType_light ipsFlex ipsFlex-ai:center ipsFlex-jc:center ipsGap_row:2 cAuthorPane_stats">
						<li>
							<!--
								<a href="https://forum.armbian.com/profile/13603-fabiobassa/content/" rel="nofollow" title="381 posts" data-ipsTooltip class="ipsType_blendLinks">
							
								<i class="fa fa-comment"></i> 381
							
								</a>
							-->
						</li>
						
					</ul>
				</li>
			
			
				<!--

	
	<li data-role='custom-field' class='ipsResponsive_hidePhone ipsType_break'>
		
<span class='ft'>Location: </span><span class='fc'>Italy</span>
	</li>
	
-->
			
		

	
	
</ul>
	</aside>
	<div class="ipsColumn ipsColumn_fluid ipsMargin:none">
		

<div id="comment-92583_wrap" data-controller="core.front.core.comment" data-commentapp="forums" data-commenttype="forums" data-commentid="92583" data-quotedata="{&quot;userid&quot;:13603,&quot;username&quot;:&quot;fabiobassa&quot;,&quot;timestamp&quot;:1578521393,&quot;contentapp&quot;:&quot;forums&quot;,&quot;contenttype&quot;:&quot;forums&quot;,&quot;contentid&quot;:34923,&quot;contentclass&quot;:&quot;forums_Topic&quot;,&quot;contentcommentid&quot;:92583}" class="ipsComment_content ipsType_medium">

	<div class="ipsComment_meta ipsType_light ipsFlex ipsFlex-ai:center ipsFlex-jc:between ipsFlex-fd:row-reverse">
		<div class="ipsType_light ipsType_reset ipsType_blendLinks ipsComment_toolWrap">
			<div class="ipsResponsive_hidePhone ipsComment_badges">
				<ul class="ipsList_reset ipsFlex ipsFlex-jc:end ipsFlex-fw:wrap ipsGap:2 ipsGap_row:1">
					
					
					
					
					
				</ul>
			</div>
			<ul class="ipsList_reset ipsComment_tools">
				<li>
					<a href="#elControls_92583_menu" class="ipsComment_ellipsis" id="elControls_92583" title="More options..." data-ipsmenu data-ipsmenu-appendto="#comment-92583_wrap"><i class="fa fa-ellipsis-h"></i></a>
					<ul id="elControls_92583_menu" class="ipsMenu ipsMenu_narrow ipsHide">
						
						
							<li class="ipsMenu_item"><a href="https://forum.armbian.com/topic/34923-csc-armbian-for-rk322x-tv-box-boards/?do=findComment&amp;comment=92583" rel="nofollow" title="Share this post" data-ipsdialog data-ipsdialog-size="narrow" data-ipsdialog-content="#elShareComment_92583_menu" data-ipsdialog-title="Share this post" id="elSharePost_92583" data-role="shareComment">Share</a></li>
						
                        
						
						
						
							
								
							
							
							
							
							
							
						
					</ul>
				</li>
				
			</ul>
		</div>

		<div class="ipsType_reset ipsResponsive_hidePhone">
			<a href="https://forum.armbian.com/topic/34923-csc-armbian-for-rk322x-tv-box-boards/?do=findComment&amp;comment=92583" rel="nofollow" class="ipsType_blendLinks">Posted <time datetime='2020-01-08T22:09:53Z' title='01/08/20 10:09  PM' data-short='4 yr'>January 8, 2020</time></a>
			
			<span class="ipsResponsive_hidePhone">
				
				
			</span>
		</div>
	</div>

	

    

	<div class="cPost_contentWrap">
		
		<div data-role="commentContent" class="ipsType_normal ipsType_richText ipsPadding_bottom ipsContained" data-controller="core.front.core.lightboxedImages">
			<p>
	<a contenteditable="false" data-ipshover="" data-ipshover-target="https://forum.armbian.com/profile/1002-jock/?do=hovercard" data-mentionid="1002" href="https://forum.armbian.com/profile/1002-jock/" rel="">@jock</a>
</p>

<p>
	Well, simply speachless .
</p>

<p>
	Great details and great work.
</p>

<p>
	Personally will support this adventure as soon as I will own more and more different boards.
</p>

<p>
	Hope someone will join the study and add ideas, suggestions, solutions
</p>

<p>
	 
</p>

<p>
	Thank you Jock
</p>

<p>
	 
</p>


			
		</div>

		
			<div class="ipsItemControls">
				
					
						

	<div data-controller='core.front.core.reaction' class='ipsItemControls_right ipsClearfix '>	
		<div class='ipsReact ipsPos_right'>
			
				
				<div class='ipsReact_reactCountOnly ipsAreaBackground_light ipsType_blendLinks ipsHide' data-role='reactCount'>
					
						<a href='https://forum.armbian.com/topic/34923-csc-armbian-for-rk322x-tv-box-boards/?do=showReactionsComment&amp;comment=92583' data-ipsTooltip title='See who reacted to this' data-ipsDialog data-ipsDialog-title='See who reacted to this'>
					
					<span data-role='reactCountText'>0</span>
					
						</a>
					
				</div>
			
			
			
		</div>
	</div>

					
				
				<ul class="ipsComment_controls ipsClearfix ipsItemControls_left" data-role="commentControls">
					
						
						
							<li data-ipsquote-editor="topic_comment" data-ipsquote-target="#comment-92583" class="ipsJS_show ipsHide">
								<button class="ipsButton ipsButton_light ipsButton_verySmall ipsButton_narrow cMultiQuote ipsHide" data-action="multiQuoteComment" data-ipstooltip data-ipsquote-multiquote data-mqid="mq92583" title="MultiQuote"><i class="fa fa-plus"></i></button>
							</li>
							<li data-ipsquote-editor="topic_comment" data-ipsquote-target="#comment-92583" class="ipsJS_show ipsHide">
								<a href="#" data-action="quoteComment" data-ipsquote-singlequote>Quote</a>
							</li>
						
						
						
												
					
					<li class="ipsHide" data-role="commentLoading">
						<span class="ipsLoading ipsLoading_tiny ipsLoading_noAnim"></span>
					</li>
				</ul>
			</div>
		

		
	</div>

	

	



<div class='ipsPadding ipsHide cPostShareMenu' id='elShareComment_92583_menu'>
	<h5 class='ipsType_normal ipsType_reset'>Link to comment</h5>
	
		
	
	
	<input type='text' value='https://forum.armbian.com/topic/34923-csc-armbian-for-rk322x-tv-box-boards/?do=findComment&amp;comment=92583' class='ipsField_fullWidth'>

	
	<h5 class='ipsType_normal ipsType_reset ipsSpacer_top'>Share on other sites</h5>
	

	<ul class='ipsList_inline ipsList_noSpacing ipsClearfix' data-controller="core.front.core.sharelink">
		
			<li>
<a href="https://www.linkedin.com/shareArticle?mini=true&amp;url=https%3A%2F%2Fforum.armbian.com%2Ftopic%2F34923-csc-armbian-for-rk322x-tv-box-boards%2F%3Fdo%3DfindComment%26comment%3D92583&amp;title=CSC+Armbian+for+RK322x+TV+box+boards" rel="nofollow noopener" class="cShareLink cShareLink_linkedin" target="_blank" data-role="shareLink" title='Share on LinkedIn' data-ipsTooltip>
	<i class="fa fa-linkedin"></i>
</a></li>
		
			<li>
<a href="https://x.com/share?url=https%3A%2F%2Fforum.armbian.com%2Ftopic%2F34923-csc-armbian-for-rk322x-tv-box-boards%2F%3Fdo%3DfindComment%26comment%3D92583" class="cShareLink cShareLink_x" target="_blank" data-role="shareLink" title='Share on X' data-ipsTooltip rel='nofollow noopener'>
    <i class="fa fa-x"></i>
</a></li>
		
			<li>
<a href="https://www.facebook.com/sharer/sharer.php?u=https%3A%2F%2Fforum.armbian.com%2Ftopic%2F34923-csc-armbian-for-rk322x-tv-box-boards%2F%3Fdo%3DfindComment%26comment%3D92583" class="cShareLink cShareLink_facebook" target="_blank" data-role="shareLink" title='Share on Facebook' data-ipsTooltip rel='noopener nofollow'>
	<i class="fa fa-facebook"></i>
</a></li>
		
			<li>
<a href="https://www.reddit.com/submit?url=https%3A%2F%2Fforum.armbian.com%2Ftopic%2F34923-csc-armbian-for-rk322x-tv-box-boards%2F%3Fdo%3DfindComment%26comment%3D92583&amp;title=CSC+Armbian+for+RK322x+TV+box+boards" rel="nofollow noopener" class="cShareLink cShareLink_reddit" target="_blank" title='Share on Reddit' data-ipsTooltip>
	<i class="fa fa-reddit"></i>
</a></li>
		
			<li>
<a href="mailto:?subject=CSC%20Armbian%20for%20RK322x%20TV%20box%20boards&body=https%3A%2F%2Fforum.armbian.com%2Ftopic%2F34923-csc-armbian-for-rk322x-tv-box-boards%2F%3Fdo%3DfindComment%26comment%3D92583" rel='nofollow' class='cShareLink cShareLink_email' title='Share via email' data-ipsTooltip>
	<i class="fa fa-envelope"></i>
</a></li>
		
	</ul>


	<hr class='ipsHr'>
	<button class='ipsHide ipsButton ipsButton_small ipsButton_light ipsButton_fullWidth ipsMargin_top:half' data-controller='core.front.core.webshare' data-role='webShare' data-webShareTitle='CSC Armbian for RK322x TV box boards' data-webShareText='@jock
 


	Well, simply speachless .
 


	Great details and great work.
 


	Personally will support this adventure as soon as I will own more and more different boards.
 


	Hope someone will join the study and add ideas, suggestions, solutions
 


	 
 


	Thank you Jock
 


	 
 
' data-webShareUrl='https://forum.armbian.com/topic/34923-csc-armbian-for-rk322x-tv-box-boards/?do=findComment&amp;comment=92583'>More sharing options...</button>

	
</div>
</div>

	</div>
</article>

					
					
					
				

					

					
					



<a id="comment-93235"></a>
<article  id="elComment_93235" class="cPost ipsBox ipsResponsive_pull  ipsComment  ipsComment_parent ipsClearfix ipsClear ipsColumns ipsColumns_noSpacing ipsColumns_collapsePhone    ">
	

	

	<div class="cAuthorPane_mobile ipsResponsive_showPhone">
		<div class="cAuthorPane_photo">
			<div class="cAuthorPane_photoWrap">
				


	<a href="https://forum.armbian.com/profile/14286-knaerzche/" rel="nofollow" data-ipsHover data-ipsHover-width="370" data-ipsHover-target="https://forum.armbian.com/profile/14286-knaerzche/?do=hovercard" class="ipsUserPhoto ipsUserPhoto_large" title="Go to knaerzche's profile">
		<img src='https://forum.armbian.com/uploads/n2a/n2a-9baf6beed4bd2ff621d175d41c8f204b2f766b63.avatars_letters.png' alt='knaerzche' loading="lazy">
	</a>

				
				
			</div>
		</div>
		<div class="cAuthorPane_content">
			<h3 class="ipsType_sectionHead cAuthorPane_author ipsType_break ipsType_blendLinks ipsFlex ipsFlex-ai:center">
				



<a href="https://forum.armbian.com/profile/14286-knaerzche/" rel="nofollow" data-ipshover data-ipshover-width="370" data-ipshover-target="https://forum.armbian.com/profile/14286-knaerzche/?do=hovercard&amp;referrer=https%253A%252F%252Fforum.armbian.com%252Ftopic%252F34923-csc-armbian-for-rk322x-tv-box-boards%252F" title="Go to knaerzche's profile" class="ipsType_break">knaerzche</a>
			</h3>
			<div class="ipsType_light ipsType_reset">
				<a href="https://forum.armbian.com/topic/34923-csc-armbian-for-rk322x-tv-box-boards/?do=findComment&amp;comment=93235" rel="nofollow" class="ipsType_blendLinks">Posted <time datetime='2020-01-18T22:12:13Z' title='01/18/20 10:12  PM' data-short='4 yr'>January 18, 2020</time></a>
				
			</div>
		</div>
	</div>
	<aside class="ipsComment_author cAuthorPane ipsColumn ipsColumn_medium ipsResponsive_hidePhone">
		<h3 class="ipsType_sectionHead cAuthorPane_author ipsType_blendLinks ipsType_break">
<strong>



<a href="https://forum.armbian.com/profile/14286-knaerzche/" rel="nofollow" data-ipshover data-ipshover-width="370" data-ipshover-target="https://forum.armbian.com/profile/14286-knaerzche/?do=hovercard&amp;referrer=https%253A%252F%252Fforum.armbian.com%252Ftopic%252F34923-csc-armbian-for-rk322x-tv-box-boards%252F" title="Go to knaerzche's profile" class="ipsType_break">knaerzche</a></strong>
			
		</h3>
		<ul class="cAuthorPane_info ipsList_reset">
			<li data-role="photo" class="cAuthorPane_photo">
				<div class="cAuthorPane_photoWrap">
					


	<a href="https://forum.armbian.com/profile/14286-knaerzche/" rel="nofollow" data-ipsHover data-ipsHover-width="370" data-ipsHover-target="https://forum.armbian.com/profile/14286-knaerzche/?do=hovercard" class="ipsUserPhoto ipsUserPhoto_large" title="Go to knaerzche's profile">
		<img src='https://forum.armbian.com/uploads/n2a/n2a-9baf6beed4bd2ff621d175d41c8f204b2f766b63.avatars_letters.png' alt='knaerzche' loading="lazy">
	</a>

					
					
				</div>
			</li>
			
				

	<li data-role="group">
		Members
	</li>







				
			
			
				<li data-role="stats" class="ipsMargin_top">
					<ul class="ipsList_reset ipsType_light ipsFlex ipsFlex-ai:center ipsFlex-jc:center ipsGap_row:2 cAuthorPane_stats">
						<li>
							<!--
								<a href="https://forum.armbian.com/profile/14286-knaerzche/content/" rel="nofollow" title="1 post" data-ipsTooltip class="ipsType_blendLinks">
							
								<i class="fa fa-comment"></i> 1
							
								</a>
							-->
						</li>
						
					</ul>
				</li>
			
			
				<!--
-->
			
		

	
	
</ul>
	</aside>
	<div class="ipsColumn ipsColumn_fluid ipsMargin:none">
		

<div id="comment-93235_wrap" data-controller="core.front.core.comment" data-commentapp="forums" data-commenttype="forums" data-commentid="93235" data-quotedata="{&quot;userid&quot;:14286,&quot;username&quot;:&quot;knaerzche&quot;,&quot;timestamp&quot;:1579385533,&quot;contentapp&quot;:&quot;forums&quot;,&quot;contenttype&quot;:&quot;forums&quot;,&quot;contentid&quot;:34923,&quot;contentclass&quot;:&quot;forums_Topic&quot;,&quot;contentcommentid&quot;:93235}" class="ipsComment_content ipsType_medium">

	<div class="ipsComment_meta ipsType_light ipsFlex ipsFlex-ai:center ipsFlex-jc:between ipsFlex-fd:row-reverse">
		<div class="ipsType_light ipsType_reset ipsType_blendLinks ipsComment_toolWrap">
			<div class="ipsResponsive_hidePhone ipsComment_badges">
				<ul class="ipsList_reset ipsFlex ipsFlex-jc:end ipsFlex-fw:wrap ipsGap:2 ipsGap_row:1">
					
					
					
					
					
				</ul>
			</div>
			<ul class="ipsList_reset ipsComment_tools">
				<li>
					<a href="#elControls_93235_menu" class="ipsComment_ellipsis" id="elControls_93235" title="More options..." data-ipsmenu data-ipsmenu-appendto="#comment-93235_wrap"><i class="fa fa-ellipsis-h"></i></a>
					<ul id="elControls_93235_menu" class="ipsMenu ipsMenu_narrow ipsHide">
						
						
							<li class="ipsMenu_item"><a href="https://forum.armbian.com/topic/34923-csc-armbian-for-rk322x-tv-box-boards/?do=findComment&amp;comment=93235" rel="nofollow" title="Share this post" data-ipsdialog data-ipsdialog-size="narrow" data-ipsdialog-content="#elShareComment_93235_menu" data-ipsdialog-title="Share this post" id="elSharePost_93235" data-role="shareComment">Share</a></li>
						
                        
						
						
						
							
								
							
							
							
							
							
							
						
					</ul>
				</li>
				
			</ul>
		</div>

		<div class="ipsType_reset ipsResponsive_hidePhone">
			<a href="https://forum.armbian.com/topic/34923-csc-armbian-for-rk322x-tv-box-boards/?do=findComment&amp;comment=93235" rel="nofollow" class="ipsType_blendLinks">Posted <time datetime='2020-01-18T22:12:13Z' title='01/18/20 10:12  PM' data-short='4 yr'>January 18, 2020</time></a>
			
			<span class="ipsResponsive_hidePhone">
				
				
			</span>
		</div>
	</div>

	

    

	<div class="cPost_contentWrap">
		
		<div data-role="commentContent" class="ipsType_normal ipsType_richText ipsPadding_bottom ipsContained" data-controller="core.front.core.lightboxedImages">
			<p>
	Hey,
</p>

<p>
	great - somebody else is working on this too - just talked about adding an armbian recpie in LE forums. I will look deeper in your work the next days (and armbian in general) - and maybe add some contribs.
</p>

<p>
	Some question beforehand:<br>
	1. How far is mainline kernel support? (Saw some commits in your repo)
</p>

<p>
	2. How about using wayland instead of X?
</p>


			
		</div>

		
			<div class="ipsItemControls">
				
					
						

	<div data-controller='core.front.core.reaction' class='ipsItemControls_right ipsClearfix '>	
		<div class='ipsReact ipsPos_right'>
			
				
				<div class='ipsReact_reactCountOnly ipsAreaBackground_light ipsType_blendLinks ipsHide' data-role='reactCount'>
					
						<a href='https://forum.armbian.com/topic/34923-csc-armbian-for-rk322x-tv-box-boards/?do=showReactionsComment&amp;comment=93235' data-ipsTooltip title='See who reacted to this' data-ipsDialog data-ipsDialog-title='See who reacted to this'>
					
					<span data-role='reactCountText'>0</span>
					
						</a>
					
				</div>
			
			
			
		</div>
	</div>

					
				
				<ul class="ipsComment_controls ipsClearfix ipsItemControls_left" data-role="commentControls">
					
						
						
							<li data-ipsquote-editor="topic_comment" data-ipsquote-target="#comment-93235" class="ipsJS_show ipsHide">
								<button class="ipsButton ipsButton_light ipsButton_verySmall ipsButton_narrow cMultiQuote ipsHide" data-action="multiQuoteComment" data-ipstooltip data-ipsquote-multiquote data-mqid="mq93235" title="MultiQuote"><i class="fa fa-plus"></i></button>
							</li>
							<li data-ipsquote-editor="topic_comment" data-ipsquote-target="#comment-93235" class="ipsJS_show ipsHide">
								<a href="#" data-action="quoteComment" data-ipsquote-singlequote>Quote</a>
							</li>
						
						
						
												
					
					<li class="ipsHide" data-role="commentLoading">
						<span class="ipsLoading ipsLoading_tiny ipsLoading_noAnim"></span>
					</li>
				</ul>
			</div>
		

		
	</div>

	

	



<div class='ipsPadding ipsHide cPostShareMenu' id='elShareComment_93235_menu'>
	<h5 class='ipsType_normal ipsType_reset'>Link to comment</h5>
	
		
	
	
	<input type='text' value='https://forum.armbian.com/topic/34923-csc-armbian-for-rk322x-tv-box-boards/?do=findComment&amp;comment=93235' class='ipsField_fullWidth'>

	
	<h5 class='ipsType_normal ipsType_reset ipsSpacer_top'>Share on other sites</h5>
	

	<ul class='ipsList_inline ipsList_noSpacing ipsClearfix' data-controller="core.front.core.sharelink">
		
			<li>
<a href="https://www.linkedin.com/shareArticle?mini=true&amp;url=https%3A%2F%2Fforum.armbian.com%2Ftopic%2F34923-csc-armbian-for-rk322x-tv-box-boards%2F%3Fdo%3DfindComment%26comment%3D93235&amp;title=CSC+Armbian+for+RK322x+TV+box+boards" rel="nofollow noopener" class="cShareLink cShareLink_linkedin" target="_blank" data-role="shareLink" title='Share on LinkedIn' data-ipsTooltip>
	<i class="fa fa-linkedin"></i>
</a></li>
		
			<li>
<a href="https://x.com/share?url=https%3A%2F%2Fforum.armbian.com%2Ftopic%2F34923-csc-armbian-for-rk322x-tv-box-boards%2F%3Fdo%3DfindComment%26comment%3D93235" class="cShareLink cShareLink_x" target="_blank" data-role="shareLink" title='Share on X' data-ipsTooltip rel='nofollow noopener'>
    <i class="fa fa-x"></i>
</a></li>
		
			<li>
<a href="https://www.facebook.com/sharer/sharer.php?u=https%3A%2F%2Fforum.armbian.com%2Ftopic%2F34923-csc-armbian-for-rk322x-tv-box-boards%2F%3Fdo%3DfindComment%26comment%3D93235" class="cShareLink cShareLink_facebook" target="_blank" data-role="shareLink" title='Share on Facebook' data-ipsTooltip rel='noopener nofollow'>
	<i class="fa fa-facebook"></i>
</a></li>
		
			<li>
<a href="https://www.reddit.com/submit?url=https%3A%2F%2Fforum.armbian.com%2Ftopic%2F34923-csc-armbian-for-rk322x-tv-box-boards%2F%3Fdo%3DfindComment%26comment%3D93235&amp;title=CSC+Armbian+for+RK322x+TV+box+boards" rel="nofollow noopener" class="cShareLink cShareLink_reddit" target="_blank" title='Share on Reddit' data-ipsTooltip>
	<i class="fa fa-reddit"></i>
</a></li>
		
			<li>
<a href="mailto:?subject=CSC%20Armbian%20for%20RK322x%20TV%20box%20boards&body=https%3A%2F%2Fforum.armbian.com%2Ftopic%2F34923-csc-armbian-for-rk322x-tv-box-boards%2F%3Fdo%3DfindComment%26comment%3D93235" rel='nofollow' class='cShareLink cShareLink_email' title='Share via email' data-ipsTooltip>
	<i class="fa fa-envelope"></i>
</a></li>
		
	</ul>


	<hr class='ipsHr'>
	<button class='ipsHide ipsButton ipsButton_small ipsButton_light ipsButton_fullWidth ipsMargin_top:half' data-controller='core.front.core.webshare' data-role='webShare' data-webShareTitle='CSC Armbian for RK322x TV box boards' data-webShareText='Hey,
 


	great - somebody else is working on this too - just talked about adding an armbian recpie in LE forums. I will look deeper in your work the next days (and armbian in general) - and maybe add some contribs.
 


	Some question beforehand: 
	1. How far is mainline kernel support? (Saw some commits in your repo)
 


	2. How about using wayland instead of X?
 
' data-webShareUrl='https://forum.armbian.com/topic/34923-csc-armbian-for-rk322x-tv-box-boards/?do=findComment&amp;comment=93235'>More sharing options...</button>

	
</div>
</div>

	</div>
</article>

					
