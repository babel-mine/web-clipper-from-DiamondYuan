Setting up a multi-boot of 5 Linux distributions | by Manu Järvinen | Medium

Clipped from: [https://medium.com/@manujarvinen/learning-some-basic-terminal-commands-d478e7b8ffe4](https://medium.com/@manujarvinen/learning-some-basic-terminal-commands-d478e7b8ffe4)

Responses (18)
--------------

#### Ben Mullen

#### over 2 years ago


---

#### 

Thank you so much! Great guide, written perfectly.

#### 

I’ve used it on different machines with different distros quite a few times now and it works perfectly every time.

#### 

Even installed Windows 10 last and it worked without screwing up Refind




---

#### 5

#### Lê Minh

#### almost 2 years ago


---

#### 

I think there’s no need to reinstall rEFInd every time a new OS installed. Just change boot sequence in BIOS Setup instead, make rEFInd as the first and everything works fine for me.




---

#### 1

#### 1

#### Atle Magnussen

#### over 2 years ago


---

#### 

A lot of good tips for installing multiple distros, thanks!

#### 

But I just noticed you are installing both ubuntu, lubuntu and xubuntu. There is no difference between those, only the default desktop environment. And for linux things are very flexible, so…...



---

#### Read More

#### 1

#### Htet Lynn Htun

#### over 2 years ago (edited)


---

#### 

Thank you so much for this amazing, jaw-dropping, awesome detailed article 😍😍



---

#### Ryan Gaudet

#### over 2 years ago


---

#### 

This was an amazing article, I needed to setup multiboot Linux box for software testing and this was pretty much a step by step installation process that worked exactly as it should.

#### 

Fantastic job!



---

#### 1

#### cmonty14

#### over 2 years ago



---

#### 

Hi,

#### 

did you ever consider to use 1 Partition with BTRFS + Subvolumes for every single Linux distribution?  
The advantage of this setup is that you don’t need to define the partition size for every single Linux OS in advance but use the “common”…...



---

#### Read More

#### Bill Turner

#### about 2 years ago



---

#### 

I am using rEFInd 0.11.2–1 and it was not necessary to re-install it after installing the second OS. Mind you, all the ones I am working with are Ubuntu based — Xubuntu, Mint-Cinnamon and Mint-XFCE — AND I am working in VirtualBox.

#### 

For each, the…...



---

#### Read More

#### Mads Nygaard

#### 11 months ago



---

#### 

Oh man… you sure got me hooked on some serious Tux tweaking Manu. Suggestion for your next Tux OS tryout would be Pop!OS. Interested if you can get this one going with the multiboot concept as it seems to utilise its own boot loader though based on…...



---

#### Read More

#### 1

#### Bruce Gray

#### over 1 year ago

#### 

1

#### 

Thanks. I also occasionally test new offerings to see how close the death of Microsoft is.

Another advantage to having more than one Linux OS, is that you can differentiate whether an issue is OS specific.



Setting up a multi-boot of 5 Linux distributions
================================================

with Windows on the side
------------------------

![](https://miro.medium.com/fit/c/56/56/0*FXmmVheEKikSZQGO.jpg)

#### Manu Järvinen

#### Jan 17, 2017·24 min read

![](https://miro.medium.com/max/2752/1*ZbUlcdw48b6TQ_OItGgZjA.jpeg)

> This article is aimed at distro hoppers who like to install multiple Linux distributions to their system drive and to be able to replace them with minimal effort when new ones come along. This is written especially digital artists and Blender users in mind. However, this article is only about the modern motherboards that have a [UEFI BIOS](http://www.basicinputoutput.com/2014/08/will-i-be-jailed-for-saying-uefi-bios.html). Required skill level: pretty much a beginner.

If you’re anything like me, about every six months you get the urge to wipe your system-disk and start from scratch and see how the latest [**Linux distributions**](http://www.howtogeek.com/132624/htg-explains-whats-a-linux-distro-and-how-are-they-different/) have developed and if _this time_ some other one suits you better than your current favorite — while trying to do all this in quick, predictable and optimized manner, not spending hours and hours for setting it all up.

I wrote this article mainly for me to remember the steps of how this was done — but perhaps it can be of some help for someone else, as well. My intention is to keep updating this article over the years as I install new distros again.

If you’re new to the Linux world, I wrote a short article for you about [**_What is Linux?_**](/@manujarvinen/what-is-linux-a80383275724)

Why a multi-boot of 5?
======================

Simply put, to try out and experiment with new constantly developing distros while still keeping the stable working favorite ones available in order to get some work done.

NVIDIA drivers
--------------

When installing a new distro, the most important thing I’m looking for is to have CUDA available in Blender’s settings as easily as possible in order to enable GPU rendering (using your graphics card to render instead of the processor, crazy fast). More about this in this article I wrote: [**_Installing NVIDIA drivers in Linux_**](/@manujarvinen/installing-nvidia-drivers-in-linux-7454f82ef5b)**_._**

Wacom tablets
-------------

Another feature I’m looking for in distros is the support for a Wacom tablet.

![](https://miro.medium.com/max/1690/1*wY7ro5vOiwG4vhyLk0ol-w.png)

Wacom Tablet control panel in Elementary OS

So far in my personal experience [**_Ubuntu_**](https://www.ubuntu.com/download/desktop), [**_Ubuntu GNOME_**](https://ubuntugnome.org/)**_,_** [**_Zorin OS (12 Beta)_**](http://zorinos.com/)**_,_** [**_Elementary OS_**](https://elementary.io/) and [**_Solus_**](https://solus-project.com/) have a good and working Wacom config panel available straight away.

However, I personally don’t see this as a huge deal breaker because I only need to map Wacom on a particular monitor and to disable the touch feature. Both of these can be handled with a couple of small startup scripts. They can be found in this article I wrote: [**_Simple Wacom scripts_**](/@manujarvinen/simple-wacom-scripts-6cd381cb320a)**_._** If you’re not familiar with the Terminal, I listed some basic commands here: [**_Learning some basic terminal commands_**](/@manujarvinen/learning-some-basic-terminal-commands-d478e7b8ffe4)**_._**

Other things
------------

Other things I’m looking for in distros are:

1.  How easily one can install up-to-date software into them
2.  The overall enjoyability of usage
3.  Performance
4.  Visual appeal

Just the basic things. Linux usage has the tendency of being quite technical and anti-user-friendly, and that can sometimes get on the way of getting some work done.

The Plan
========

My plan was to have 5 different Linux distributions on my system and to be able to ditch and replace any of them without affecting any of the others or the boot manager.

![](https://miro.medium.com/max/3840/1*fH47zz7ZWM3HU2tap6teGg.jpeg)

The Plan: the Refind boot manager and some easily switchable distros.

In addition, I keep a [**handy Workflowy list**](https://pbs.twimg.com/tweet_video/CG2kKy6UgAAx82h.mp4) of my favorite terminal commands and installing instructions and a Dropbox account with my application settings helping to set up a new distro in no time.

I also had to put a Windows 7 partition into the disk in order to play some games (like [**_INSIDE_**](http://store.steampowered.com/app/304430/), from the makers of Limbo :) — Installing Windows 7 is surprisingly easy, works well with the Refind boot manager and doesn’t mess it up. In the case of Windows _10_, though, I remember reading somewhere it should be installed first because installing it wipes out the entire hard disk — haven’t tested it, though. So, can’t say. Comments about this are welcome :)

Furthermore, I didn’t want a shared _/home_ partition because problems started to emerge when I was constantly switching distros and different settings were overwriting each others in /home. Also in some distros I like trying out new stuff, experimenting and tweaking a lot, and those settings should affect only that particular distro, not the others.

My typical behavior is to save any files I work on away from where the operating system was installed. I use separately made **/work** and **/storage** partitions for that. That’s so that I can freely wipe the OS partition at any time without having the feeling that I have left anything important there.

More about my working methods and backing-up/archiving habits in [**_this separate article I wrote_**](/@manujarvinen/my-method-for-storing-files-49bf71c6edbd).

1\. Preparation
===============

Without knowing any better, for my previous configuration I had just thrown my system SSD hard drive onto a random SATA port in the motherboard. That caused my secondary 3TB storage drive to be displayed as the first disk in my UEFI BIOS instead of the main SSD system disk. So when it was time to partition the system disk and install an operating system into the SSD, it was seen as **_/dev/sdb_** (the second hard drive in the computer) instead of **_/dev/sda_** (the first hard drive in the computer). It was a bit annoying, even though it was pretty much only a cosmetic thing.

> The “dev” in /dev/sda means the devices directory. “Sda” [means **S**CSI **d**isk A](http://www.tldp.org/HOWTO/Partition-Mass-Storage-Definitions-Naming-HOWTO/x99.html).

![](https://miro.medium.com/max/4902/1*iqg-FZLmg-4rWNq6c9jyRw.jpeg)

Upper image: SATA port number one in motherboard’s manual. Lower image: in UEFI BIOS

So, **firstly**, I made sure to put the SSD into the physical SATA port _number one_ in the motherboard.

Upper image on the left is from the motherboard’s manual, lower image from the boot sequence, showing the SSD disk being in SATA port _number one_.

![](https://miro.medium.com/max/2000/1*jiigGd1lxFQA1yT4asX1cA.jpeg)

**Secondly**, in the UEFI BIOS I made sure the system disk is the first one to be booted (one of those “INTEL SSD…” choices).

![](https://miro.medium.com/max/8064/1*YTRqLci9FCzbkCkigmIqDQ.jpeg)

**Thirdly**, in UEFI BIOS there was this _‘Secure boot’_ option that had to be switched to the **_‘Other OS’_** option in order to install something other than only Windows to the computer.

Then, I download and check the .iso’s ([**_Here_**](/@manujarvinen/downloading-and-verifying-iso-images-960f75c4d6be)’s an article I wrote about that). And after that I make some bootable USB sticks ([**_Here_**](/@manujarvinen/making-bootable-usb-sticks-cc60b52354b2)’s an article I wrote about that).

![](https://miro.medium.com/max/7384/1*EqsUWIjgVicAFOsvxzPBrQ.jpeg)

Bootable distro USB sticks

2\. Installing the first OS and the Refind boot manager
=======================================================

I used Xubuntu as the first operating system on my system disk. I’ve used it a lot, I’m very familiar with it and it’s one of my favorite distros. It’s also Ubuntu-based, which makes it easy to run the Refind boot manager’s installer [**_.deb_**](https://en.wikipedia.org/wiki/Deb_(file_format)) file whenever needed.

**2.1 Create a new GPT partition table for the whole physical system disk**
---------------------------------------------------------------------------

I wanted to use [**_Refind_**](http://www.rodsbooks.com/refind/) as the boot manager for my system. For it to work, the system disk needs to have a modern [**_GUID partition table (GPT)_**](https://en.wikipedia.org/wiki/GUID_Partition_Table) instead of the old [**_MBR partition table_**](http://www.disk-partition.com/articles/mbr-partition-table-to-gpt.html) (_‘msdos’_ in GParted). For modern UEFI BIOS motherboards and especially for 2 TB or larger hard drives there is no sense in using MBR anymore pretty much at all, as far as I know. Please, correct me if I’m wrong, feedback is appreciated :)

**Naturally, you should have all your data backed up either on another physical disk or an external hard drive at this moment.**

I’m using a partition utility called **GParted** for handling the system hard drive partitioning. It comes nicely bundled in Xubuntu.

![](https://miro.medium.com/max/1866/1*5_gAmaF3yMmF8s_dl_V6nQ.jpeg)

UEFI BIOS boot menu, by pressing F8 during boot. Selecting the USB stick.

![](https://miro.medium.com/max/1200/1*4dFOlaFVm47_hbbRDVTigQ.jpeg)

So first, boot from the Xubuntu USB stick. Pick the **_‘Try Xubuntu without installing’_** option.

![](https://miro.medium.com/max/754/1*bOQygMREbrs0KbO_Io5IrA.jpeg)

**Ctrl + Esc** opens up the Whisker menu in Xubuntu

![](https://miro.medium.com/max/1400/1*oZuwsfaqJfVepxpYg8T0EQ.jpeg)

Then, in Xubuntu, I press **Ctrl+Esc** to open the Whisker-menu, type **GParted** and launch it.

I had some kind of a notification when opening GParted, which I decided to ignore.

I then create a new GPT partition table for the whole system disk from the menu **_“Device”_** \> **_“Create Partition Table …”_** . There’s no need to delete the previous system partitions, because creating a new partition table will clear _everything_ in that physical disk, all the partitions at once. A good way to start from scratch. Everything will be deleted. (Again, remember to do your backups before doing this)

![](https://miro.medium.com/max/3342/1*IQeiftaE9lDGS0g1oq03iQ.jpeg)

Wiping the whole system disk and creating a new GPT partition table for it.

![](https://miro.medium.com/max/2000/1*IEq2PNw-WepqEB9NhvjbWg.jpeg)

The whole slate is clean!

2.2 Partitioning
----------------

The most important step here is to make the first partition (/dev/sda1) as an **EFI System Partition (ESP)**. This is where the brains of the system booting will eventually go, the Refind boot manager.

![](https://miro.medium.com/max/8064/1*dhfedZ7sXbX25kZoqjHN4A.jpeg)

Deepin needed more than 100MB for the EFI System Partition

In my first tests I made it less than 100 MB in size, but on my later partition setup I made it 1000 MB because I noticed some distros like [**Deepin**](https://www.deepin.org/?language=en) need more than 100 MB for the EFI partition. 1 MB of free space will be created automatically out of the first partition you create. It’s for SSD alignment, which is important for keeping the SSD speeds up, I was once told. The second **bios boot partition** was required by some distros, like [**Fedora**](https://getfedora.org/). I left it unformatted for now, but in future I might need it if I install Fedora. It could’ve been left way smaller, like even 1 MB (see below the Fedora message image), but I went with 1000 MB, just in case. Naturally, this can be also resized afterwards, if needed.

Here’s the partitioning I used for my SSD system disk. I also labeled the partitions for clarity:

![](https://miro.medium.com/max/2200/1*KEmlfr0d8aOljzD2IHw0Ww.png)

Partitioning done, ready to apply all operations

Currently the partition number three is for swap. Normally you might not even need it, but, for example, if I happen to make way too heavy 3D particle grass fields or sculpts that use more RAM than 32 GB to render, I am definitely going to need it. It’s easy to expand it larger later if needed.

![](https://miro.medium.com/max/15356/1*cM87nLnyJ0c88ehSeo1HrA.jpeg)

Fedora required the biosboot type of partition. Swap also seemed to be mandatory.

In my previous tests, it also seemed Fedora specifically _required_ a swap partition in order to continue its installation.

However, now when I think of it on a later thought, Windows might have been better on the partition number 4, because it won’t be getting moved, like I often do with the distro partitions. Also creating more partitions for more distros to the end of the SSD wouldn’t cause any problems for the partition numbering.

I created 3 extra 100 MB partitions (_#9_, _#10_ and _#11_) if I want to install more than five Linuxes to the system. I just expand one of them and install into it. I often move, expand and shrink my partitions on the go afterwards (it surprisingly has always worked, even), but never change their ordering or create new ones into the middle, that might cause some serious issues. More about this in the **section 10** of this article.


![](https://miro.medium.com/max/1900/1*BipCoDzV_6gZExMtCz7VXg.jpeg)

Installation failed because of not putting the **boot flag** for the first EFI BOOT partition

When the partitioning was done, it was crucial to remember to mark the **boot flag** for the first EFI boot partition before installation (the **esp flag** gets automatically marked as well), otherwise the install fails, like it did for me (image on the left).

I also marked the unformatted second partition with a **bios_grub** flag for Fedora. Not sure if this was needed, though.

![](https://miro.medium.com/max/2434/1*rIGl2ybL5jf-usrEz7Xb6w.jpeg)

Adding the **boot flag** and **bios_grub** flag to the partitions #1 and #2

After applying all the operations, it was time to start the installation of Xubuntu.

![](https://miro.medium.com/max/2430/1*34_bJzyWMeWn-TsiwlKiHg.png)

Applying all operations and starting the installing of Xubuntu

2.3 Installing Xubuntu
----------------------

During the installation of Xubuntu, I made mount points for the to-be-installed Xubuntu and for the Work and Storage partitions:

*   **/** ( _/dev/sda4, EXT4_ )
*   **/work** ( _/dev/sda12, 25 GB EXT4 in the SSD_)
*   **/storage** ( _/dev/sdb1, 3 TB NTFS hard disk_ )

I wrote here more detailed instructions for those who would like to have them: [**_Installing Xubuntu_**](/@manujarvinen/installing-xubuntu-efd84051b3dd)**_._**

2.3.1 Temporary boot loader
---------------------------

For the **_‘Device for boot loader installation’_** section I always will choose **_/dev/sda_** in all of my distro installations_._

![](https://miro.medium.com/max/2600/1*6OC_4N17wQ1SJdIWgksV5Q.jpeg)

![](https://miro.medium.com/max/2600/1*6OC_4N17wQ1SJdIWgksV5Q.jpeg)

It’s going to be used for the first boot but after that it’ll be overruled by installing the Refind boot manager.

Then I continued the installation normally and finished it.

**2.4** User rights to write to the Work and Storage partitions
---------------------------------------------------------------

After the installation was finished, I noticed I can’t make folders or files to the root of the /storage or /work partitions. So, I went to the newly installed Xubuntu and started a terminal window via **_Ctrl+Alt+T_**. In some other Linuxes the terminal shortcut might be something else.

I used the command **_chown \[user\] \[location\]_** to give the user rights to write to the mount-points, so that I can create folders and files straight to the root of the drives:


\# See what's your user ID (for me it's _mj_):  
**id -u -n**  
\# Give write rights for the user:  
**sudo chown mj /storage/**  
**sudo chown mj /work/**  
\# A single chained command might work also to do the same thing:  
 **sudo chown mj /storage/ /work/**


If you want to know more about my ways of configuring, I wrote a separate article about [**_setting up Xubuntu after installation_**](/@manujarvinen/setting-up-xubuntu-after-installation-b824080e1f51).

2.5 Installing the Refind boot manager
--------------------------------------

A bit about boot managers’ and a boot loaders’ differences. From Refind’s [**_webpage_**](http://www.rodsbooks.com/refind/):

> _”Refind is a_ **_boot manager_**_, meaning that it presents a menu of options to the user when the computer first starts up, as shown below. Refind is_ **_not a boot loader_**_, which is a program that loads an OS kernel and hands off control to it.”_
> 
> “Since version 3.3.0, the Linux kernel has included a built-in _boot loader_ and that the older GRUB that many of us are used to use is both a _boot manager_ and a _boot loader.”_

Download the latest version (0.10.4–1 at the time of writing this article) of Refind **.deb** file from [**_here_**](https://sourceforge.net/projects/refind/files/)**_._**

Or from command-line:


---
\# Downloading refind\_0.10.4-1\_amd64.deb to the Downloads folder in Home (~/Downloads):**wget -P ~/Downloads/** [**https://downloads.sourceforge.net/project/refind/0.10.4/refind\_0.10.4-1\_amd64.deb**](https://downloads.sourceforge.net/project/refind/0.10.4/refind_0.10.4-1_amd64.deb?r=https%3A%2F%2Fsourceforge.net%2Fprojects%2Frefind%2Ffiles%2F0.10.4%2F&ts=1484667353&use_mirror=vorboss)

---

For copy-pasting these commands easily, you can either normally select the command via mouse cursor or you can **_triple-click_** the command (or somewhere in the gray area) to select it all, then **_Ctrl+C_** and **_Ctrl+Shift+V_** it to the **_terminal_**.

![](https://miro.medium.com/freeze/max/60/1*OQlmp3LaVBHleyYc_cwx5w.gif)

Triple-clicking to select a command, **Ctrl+C** to copy it to the clipboard and then **Ctrl+Shift+V** to paste it into the Terminal

By the way, did you know [**_this_**](http://i.imgur.com/baH6YEj.gif) about triple-clicking?

This will download the _refind\_0.10.4–1\_amd64.deb_ to your _Downloads_ directory. Download a different one for your non-Debian based system.

Install it with a command:


---
**sudo dpkg -i ~/Downloads/refind\_0.10.4-1\_amd64.deb**

---

![](https://miro.medium.com/max/1926/1*zm2ngh2l1CnIAY4G1NAggg.png)

Installing Refind

And so, Refind is installed.

**2.6 Finding a theme for the Refind boot manager**
---------------------------------------------------

Here are some Refind themes I found from GitHub. Before installing, check from the link if it has all the icons for the distros you’re going to use.

![](https://miro.medium.com/max/3840/1*nqmSBZcxZNU0loXhKvwlpA.png)

[**_Refind Black_**](https://github.com/anthon38/refind-black)

![](https://miro.medium.com/max/2048/1*lrgzWQUQVC6G7Px6nF1yFw.jpeg)

[**_Refind Black Theme_**](https://github.com/st-andrew/rEFInd-Black)

![](https://miro.medium.com/max/2560/1*rZub2_b6UgARBH-kJGWQtg.jpeg)

[**_Dream Machine_**](https://github.com/Lindstream/dm-refind-theme)

![](https://miro.medium.com/max/3200/1*zIs3uTEBBXbUwslzmAmubg.png)

[**_Numix Circle_**](https://github.com/abazad/refind-numix-circle)

![](https://miro.medium.com/max/3840/1*TUHEqCtYrjFtsZlGr2FW5A.png)

[**_Refind Minimal_**](https://github.com/EvanPurkhiser/rEFInd-minimal)

![](https://miro.medium.com/max/5120/1*srXLmgALQRwd411prhhYEA.png)

[**_Ambience_**](https://github.com/lukechilds/refind-ambience)

![](https://miro.medium.com/max/2732/1*YNn4zEk7NQXB_y24DXmPjQ.png)

[**_Darkmini_**](https://github.com/LightAir/darkmini)

![](https://miro.medium.com/max/1708/1*HJ0KvIrHKITb91shpulHhA.png)

[**_Regular_**](https://github.com/munlik/refind-theme-regular)

I use the above theme [**_Regular_**](https://github.com/munlik/refind-theme-regular) for this example.

2.7 Installing the theme Regular for the Refind boot manager
------------------------------------------------------------


---
\# Install git:  
**sudo apt install git**

\# Get rights to enter the EFI folder (because some distros prevent you going there   
(Note, also could work: _sudo bash_ or _su_ or _sudo -s_)) :  
**sudo -i**

\# check if sda1 is mounted to /boot/efi  
**lsblk**

---

By doing the above lsblk you can see if the sda1 partition has been given the mount point /boot/efi.

If it is, go to the next step about deleting. If not, then:


---
\# make an efi folder (if it isn't already there)  
**mkdir /boot/efi/**

\# Mount /dev/sda1 to /boot/efi  
**mount /dev/sda1 /boot/efi/**

---

And continue with the process:


---
\# Delete possible older installed versions of this theme (always be extra careful when using the **rm** (remove files) command with Root rights, accidental deleting of important stuff is not fun):  
**rm -rf /boot/efi/EFI/refind/{regular-theme,refind-theme-regular}**

\# Fetch the Regular theme:  
**git clone https://github.com/munlik/refind-theme-regular.git /boot/efi/EFI/refind/refind-theme-regular**

\# Remove unused stuff:  
**rm -rf /boot/efi/EFI/refind/refind-theme-regular/{src,.git}**

---

Configure Refind:


---
\# Use the command line interface (CLI) text editor Nano to edit the Refind's configure file:  
**nano /boot/efi/EFI/refind/refind.conf**

\# Use Ctrl+W to search for this sentence:  
**resolution 3**

\# Under that line, write:  
**resolution 1600 1200**

\# (For me, 1600 1200 was the maximum. You can try larger resolutions if you wish. When you boot, it will tell you the accepted choices for your system. After that you can come back to edit this file again.)  

\# Hit Page Down until you reach the end of the document.  
\# Paste (Ctrl+Shift+V) the following line there:  
**include refind-theme-regular/theme.conf**

\# Quit Nano with Ctrl+X.  
\# Save the current document by pressing Y and Enter.  

\# If you made any mistakes for some reason (there's no undo as far as I know), it's recommendable to quit Nano with Ctrl+X and answering no and starting from the beginning.  

\# To adjust icon size and font size edit theme.conf (however, the default size is quite nice already):   
nano /boot/efi/EFI/refind/refind-theme-regular/theme.conf  

\# Exit the Root session:  
**exit**

---

If you installed Xubuntu as the first operating system, in the _/boot/efi/EFI_ there’s an extra boot option called _‘ubuntu’_. This is something that was probably installed during Xubuntu’s installation. This will annoyingly appear in the boot manager next to the Xubuntu boot choice after you reboot. And using it seems to go to the same Xubuntu as the Xubuntu icon (mouse) one:

![](https://miro.medium.com/max/2600/1*Nwf0Sa0UedddzcmE1LbCuQ.jpeg)

Refind boot menu with annoying extra ‘ubuntu’ choice and some pink firmware update icon

You can move this _‘ubuntu’_ option and the pink firmware update icon away via:


---
\# Open the Thunar file manager with Root rights:  
**sudo thunar**

---

![](https://miro.medium.com/max/1280/1*w59JE80-rWN5MPFUWy_WCg.png)

Moving **‘ubuntu’** folder into **IGNORE** folder in **/boot/efi/EFI/**

And then pressing **_Ctrl+L_** to go to the address bar. Type:


---
/boot/efi/EFI/

---

By pressing **_Ctrl+Shift+N_** make a folder called “IGNORE”, for example. Move the _‘ubuntu’_ folder into it.

Close the file manager.  
Reboot the computer.  
You can do it normally from the menus, or use terminal command for that:


---
\# Reboot the computer (It’s instant, make sure you don’t have anything important or unsaved open):  
**sudo reboot**

---

During the boot, you should see the following:

![](https://miro.medium.com/max/2600/1*mtVrLDLyIZem_kSdPVj_3A.jpeg)

Refind boot manager installed.

3\. Installing the second operating system
==========================================

Ubuntu GNOME was waiting next in the distro line to get in:

![](https://miro.medium.com/max/2200/1*pXhowZfga_9BaguFnzON1w.jpeg)

Setting up installation mount point for Ubuntu GNOME

I install Ubuntu GNOME (mount point: **/** ) into **_/dev/sda5_**. I remember to give mount points to **_/work_** (ext4)  and **_/storage_** (NTFS) like mentioned in the 2.3 section of this article. Be careful **_not_** to mark them as to be formatted.

The Swap partition gets automatically recognized, no need to worry about that.

![](https://miro.medium.com/max/2600/1*q8RcSxPyhcwulrXxDHgJHA.jpeg)

Weird USB stick error. It happened after Ubuntu GNOME installation had been successfully finished, but the computer refused to reboot.

When the Ubuntu GNOME installation was finished and it was time to reboot — suddenly this appeared (some error about the USB stick) and didn’t go away. I was forced to press the reset button on the computer. However, Ubuntu GNOME was installed successfully without further problems.

After the reboot I go into the first distro, **_Xubuntu_** (/dev/sda4), again:

![](https://miro.medium.com/max/2600/1*ktkysqHlCaxKNcFjFxY_hA.jpeg)

Choosing Xubuntu in Grub boot menu. It’s confusingly named as Ubuntu.

I open terminal and install Refind again (by pressing **_up arrow_** I can find the command that was used before):


---
**sudo dpkg -i ~/Downloads/refind\_0.10.4-1\_amd64.deb**

---

![](https://miro.medium.com/max/1284/1*eq-aQQc5Us1L9BYMQWLuGQ.png)

I was once noted that correct method really isn’t to install Refind boot manager on top of a older Refind installation _—_ but, since I don’t know any better, I kind of find it easier to just let it do what it does in order to make the system work again than to figure out the right way to configure it for that. Refind installer even recognizes the customizations and configurations I did before:

![](https://miro.medium.com/max/1532/1*78wisSjjvDa42mWx2FE0fA.png)

Refind installer recognizing the customizations and configurations on re-install

If you happen to know the **simpler and more correct** way of making Refind to work after installing a new distro, please give a comment and I’ll edit it into the article :)

So, after re-installing Refind:


---
**sudo reboot**

---

![](https://miro.medium.com/max/2600/1*VDWAZOqCH1jvDGnCkenDzg.jpeg)

There’s the extra ‘ubuntu’ choice again (left Ubuntu icon) and Refind shows Ubuntu GNOME as normal Ubuntu (right Ubuntu icon)

So, there’s that ‘ubuntu’ choice again and somehow in the boot manager Ubuntu GNOME’s icon showed as a normal Ubuntu icon, probably Refind doesn’t recognize the Ubuntu GNOME from a regular Ubuntu. (You can see it points to the Ubuntu GNOME partition in the boot manager text in the image above.)

**Solution:** You can kindly force Refind to use a specific icon for an OS partition by putting it onto the partition’s root with a name **_.VolumeIcon.png_**:


---
\# Check what the ubuntu gnome´s partition is with GParted, for example. In this case, sda5\# Mount Ubuntu GNOME's sda5 partition:  
**sudo mkdir /mnt/ubuntugnome**  
**sudo mount /dev/sda5 /mnt/ubuntugnome**

\# Copy the Ubuntu GNOME icon to the root of sda5:  
**sudo cp /boot/efi/EFI/refind/refind-theme-regular/icons/128-48/os_ubuntugnome.png /mnt/ubuntugnome/.VolumeIcon.png**

\# Unmount:  
**sudo umount /mnt/ubuntugnome**

---

And once again, let’s move the _‘ubuntu’_ folder away:


---
\# Open the Thunar file manager with Root rights:  
**sudo thunar**

---

And go to /boot/efi/EFI to move the _‘ubuntu’_ folder into the IGNORE folder.

![](https://miro.medium.com/max/2376/1*C3R8pD8Nma1LdzfX5Jbp-w.png)

Overwriting files in /boot/efi/EFI/IGNORE/

I choose to just overwrite the files in the IGNORE folder — because they seem to be identical (exactly same sizes).

Then, I close the file manager and:


---
**sudo reboot**

---

And voilà:

![](https://miro.medium.com/max/2600/1*-ST1pm4xUoOSuAhGDy1gcw.jpeg)

4\. Installing the third operating system
=========================================

Manjaro XFCE’s turn.

![](https://miro.medium.com/max/2600/1*-dVAf6y4H2QWQ1FX5xnNAg.jpeg)

Manjaro has many installer choices, I like the _Calamares_ one (best to view large amount of partitions when installing).

I install Manjaro XFCE to **_/dev/sda6_** partition. And once again, I remember to give mount points to **_/work_** (ext4) and **_/storage_** (NTFS) and to **_NOT_** format them.

And, the Swap partition gets automatically recognized again.

Manjaro didn’t even mess up the Refind boot manager, it appeared nicely into the menu automatically:

![](https://miro.medium.com/max/2600/1*MSg5FqBfQBFA5cgpJlE1lA.jpeg)

5\. Installing the fourth operating system
==========================================

Time to install Lubuntu to **_/dev/sda7_**:

![](https://miro.medium.com/max/2600/1*ZSM7AcTSXZeev34Zq7LqTQ.jpeg)

Mount points also to **_/work_** (ext4) and **_/storage_** (NTFS), as usual.

And, the Swap partition gets automatically recognized again.

After installation was successfully finished, I clicked the **_restart now_** option in the Lubuntu installer. It went to black screen but nothing happened, I had to press reset button on the computer :/

![](https://miro.medium.com/max/2600/1*r2ftwBGb2lwHYE2ZfYAtzA.jpeg)

On my way to the Xubuntu (/dev/sda4) option after installing Lubuntu. It’s still confusingly named as Ubuntu.

And once again after installation I reboot the computer, go to Xubuntu and in Terminal:


---
**sudo dpkg -i ~/Downloads/refind\_0.10.4-1\_amd64.deb**  
**sudo reboot**

---

![](https://miro.medium.com/max/2600/1*W45EyHH7DPYJYpl4uBBl9Q.jpeg)

After that I once again move the ‘ubuntu’ folder in **/boot/efi/EFI** to the **IGNORE** folder and overwrite everything:


---
**sudo thunar**

---

![](https://miro.medium.com/max/2376/1*C3R8pD8Nma1LdzfX5Jbp-w.png)


---
**sudo reboot**

---

6\. Installing the fifth operating system
=========================================

Kubuntu.

In its installer, Kubuntu has the most well done partition view I have ever seen, you can even resize it to see the partitions all at once:

![](https://miro.medium.com/max/2800/1*84vuNh-7P8lmDV5yxuucEA.jpeg)

I install Kubuntu ( **/** ) to **_/dev/sda8_**. Mount points also to **_/work_** (ext4)  and **_/storage_** (NTFS), again.

And, the Swap partition gets automatically recognized, again. And finally, after installation is successfully finished, one last time to install the Refind boot manager in Terminal in Xubuntu:


---
**sudo dpkg -i ~/Downloads/refind\_0.10.4-1\_amd64.deb**

---

![](https://miro.medium.com/max/2600/1*yiwLI2UElnUFBRL0xyYQTw.jpeg)

Going back to Xubuntu (/dev/sda4) after installing Kubuntu

And done:

![](https://miro.medium.com/max/8064/1*JSy4kBkJaT3AlRV4QZNY4w.jpeg)

7\. Installing Windows 7
========================

Then I install windows 7 to the partition 13.

![](https://miro.medium.com/max/2600/1*bWXPt7-apepiXsgU-s1etQ.jpeg)

The Windows 7 DVD

![](https://miro.medium.com/max/2600/1*EaMi1fLLGiiIXFtVp_7kEg.jpeg)

Installing Windows

![](https://miro.medium.com/max/2600/1*Rci8BymDFaG8qIQ96xD48Q.jpeg)

Service Pack 1 and **_Microsoft Security Essentials_**
------------------------------------------------------

If you need to get the Windows 7 Service Pack 1, like I did (in order to play Steam games), they’ve really seen effort to hide it [**_here_**](https://www.microsoft.com/en-us/download/details.aspx?id=5842) and the correct file you want to download (for Windows 7 64-bit) after pressing the red download button is named: **_windows6.1-KB976932-X64.exe_** — very convenient, eh?

It’s also a good idea to install the free [**_Microsoft Security Essentials_**](https://support.microsoft.com/en-us/help/14210/security-essentials-download)**_._**

After installing Windows, it might _seem_ like it has done something to the boot managers, because it now boots straight into Windows and you can’t see either the old Grub boot manager or Refind at all.

However, if you give a closer look to your **_UEFI BIOS settings_**, Windows has sneakily changed the first boot option for himself, leaving Refind second. Change the first boot option back to Refind and everything looks perfectly like it should! :)

![](https://miro.medium.com/max/3000/1*jZjhzn9zKz-vgje7JcuDwA.jpeg)

Changing boot option number one from **Windows** to **Refind** in the UEFI BIOS

![](https://miro.medium.com/max/2600/1*VyT8FvwKmgwAaPpj1JHbyw.jpeg)

Clean boot manager once again!

8\. Making a Refind rescue USB stick
====================================

This is totally optional, but it’s handy to have a USB Refind rescue stick for any distro hopper. There can be times when you need to open the Refind boot manager menu if something has happened to the original one.

You can download the Refind USB flash drive image file from this [**page**](http://www.rodsbooks.com/refind/getting.html) or by just clicking [**here**](https://sourceforge.net/projects/refind/files/0.10.4/refind-flashdrive-0.10.4.zip/download) (version 0.10.4).

Instructions on how to write it to the USB stick are inside the page, but shortly in Linux it’s just:


---
\# Check what's your USB sticks name (ie. /dev/sdc)  
**lsblk**

\# Writing refind-flashdrive-0.10.4.img to /dev/sdc:  
**sudo dd if=refind-flashdrive-0.10.4.img of=/dev/sdc**

---

In Windows you can use [**_this_**](https://rufus.akeo.ie/) utility.

I used these settings:

![](https://miro.medium.com/max/890/1*0KNIjViwNUUfhbBCvXOrHA.jpeg)

Writing Refind USB flash drive image file to a USB stick

![](https://miro.medium.com/max/2600/1*lDLYgsXmbhI74fWf4K9GmA.jpeg)

Using the Refind USB Rescue from the USB stick

It looks awful, but can be a savior. From there you can easily go back to Xubuntu for example to install the **_Refind .deb installer_** again in order to recover things.

9\. Tweaks
==========

If you want to add the small gray arrow seen in the following Refind image below (and a circle image for selecting the small icons in Refind), do in Xubuntu:


---
\# Get the arrow image. Backup the original selection-big.png as selection-big_original.png. Put arrow image in its place:  
**sudo wget http://i.imgur.com/Kax1bn7.png -P /boot/efi/EFI/refind/refind-theme-regular/icons/128-48/ && sudo mv /boot/efi/EFI/refind/refind-theme-regular/icons/128-48/selection-big.png /boot/efi/EFI/refind/refind-theme-regular/icons/128-48/selection-big_original.png && sudo mv /boot/efi/EFI/refind/refind-theme-regular/icons/128-48/Kax1bn7.png /boot/efi/EFI/refind/refind-theme-regular/icons/128-48/selection-big.png**

\# Get the small circle image. Backup the original selection-small.png as selection-small_original.png. Put small circle image in its place:  
**sudo wget http://i.imgur.com/odu8ol2.png -P /boot/efi/EFI/refind/refind-theme-regular/icons/128-48/ && sudo mv /boot/efi/EFI/refind/refind-theme-regular/icons/128-48/selection-small.png /boot/efi/EFI/refind/refind-theme-regular/icons/128-48/selection-small_original.png && sudo mv /boot/efi/EFI/refind/refind-theme-regular/icons/128-48/odu8ol2.png /boot/efi/EFI/refind/refind-theme-regular/icons/128-48/selection-small.png**

---

Restart:


---
**sudo reboot**

---

You should see this:

![](https://miro.medium.com/max/2600/1*z5dfC2Yx9LeBIoFNpyGu_A.jpeg)

Final 5-way multi-boot achieved! (And Windows)

For the last little tweak, I went to GParted and it seemed the Labels of the partitions had been blanked during the distro installations. So, I went and put labels on them again. **/work** and **/storage** partitions needed to be unmounted (right-click > unmount) before I could label them. Xubuntu couldn’t be unmounted because that’s the distro we were using, so I labeled Xubuntu in GParted in Manjaro later.

![](https://miro.medium.com/max/2600/1*4iDMLFUuqNVq9Wmi2UPY2A.jpeg)

That’s it :)

There might be more sensible way of doing all of this, but so far this is the best method I’ve come up with for myself. I will be updating this article later when I become more knowledgeable of installing different distros.

Every time I switch to a new distro, this article serves as a good reminder for me how easily I can swap a random OS to another like [**_OpenSUSE_**](https://www.opensuse.org/), [**_CentOS_**](https://www.centos.org/), [**_Solus_**](https://solus-project.com/), [**_Deepin_**](http://www.deepin.org/?language=en), [**_Antergos_**](https://antergos.com), [**_KaOS_**](https://kaosx.us/), [**_ChaletOS_**](https://sites.google.com/site/chaletoslinux/), [**_KDE Neon_**](https://neon.kde.org/), [**_CrunchBang++_**](https://crunchbangplusplus.org/)**_,_** [**_Archbang_**](http://wiki.archbang.org/index.php?title=Main_Page)**_,_** [**_Apricity OS_**](https://apricityos.com)**_,_** [**_VeltOS_**](https://velt.io/), [**_Ubuntu Budgie_**](https://budgie-remix.org/), [**_Linux Mint_**](https://linuxmint.com/) _or_ [**_Endless OS_**](https://distrowatch.com/table.php?distribution=endlessos), for example.

Not to mention [**_Lakka_**](http://www.lakka.tv/)**_,_** [**_SteamOS_**](http://store.steampowered.com/steamos/)  or [**_OSMC_**](https://osmc.tv/)  for the media center computer (OSMC even allows running a SSH server on the background with irssi IRC client open in screen that you can connect to from your phone from anywhere. Although I prefer [**_Discord_**](https://discordapp.com/) for chatting (both in desktop and mobile) nowadays. I can of course write a separate article about irssi IRC and OSMC if there’s any demand for it). In future, I might also try the [**_Plasma Mobile_**](https://plasma-mobile.org/), [**_Ubuntu Phone_**](https://www.ubuntu.com/phone) or [**_Sailfish OS_**](https://sailfishos.org/) for the mobile phone or tablet.

I keep trying and testing until I find my next favorite.

Alternatively, you can even install [**_Mac OS X on your PC_**](https://www.youtube.com/watch?v=OMLCJ0JMK9Q), if you so wish.

10\. Resizing partitions
========================

![](https://miro.medium.com/max/2638/1*oFexID1QZDP2xxLkuwSsSg.png)

Cropped screenshots from GParted

Every now and then I keep re-sizing the partitions if I have run out of space on one or just for the sake of testing. It only takes a couple of minutes to perform the resizing.

You can’t resize a partition you’re currently using, so boot from a distro whose partition you’re not going to be modifying or boot from the Xubuntu USB stick, for example, and pick the **_‘Try Xubuntu without installing’_** option and open GParted there.

During tweaking the partitions, GParted warns about _“Moving a partition might cause your operating system to fail to boot”._ I can’t remember a single occasion where this would’ve resulted in an unbootable system for me. I used to fear doing this but not anymore. I just make sure **_not to delete or create new partitions in the middle of the partition array in order to keep the partition numbering correct_**. After that all the systems still boot nicely.

![](https://miro.medium.com/max/2570/1*Mv84P9GJTFO-ONpknK5Usg.jpeg)

Shrinking and moving a lot of partitions and expanding one in GParted

Applying all the operations take just a handful of minutes even for large moving and resizing operations:

![](https://miro.medium.com/max/1944/1*LQmNdzDBfBZDwoekKyuT-g.jpeg)

Applying all operations in GParted

**11\. Notes after installing different distros**
=================================================

Here are some notes from my earlier distro experiments, like installing [**_OpenSUSE_**](https://www.opensuse.org/) or [**_Deepin_**](https://www.deepin.org/en/?language=en).

It might make sense to install OpenSUSE on an EXT4 partition first if you’re only testing it. By default it recommends BTRFS file system instead of EXT4 which seemed to [**_make all kinds of volumes and subvolumes_**](https://tweakhound.com/2014/11/13/dual-boot-opensuse-13-2-and-windows-8-1-uefi/) that probably are the ‘right’ way of installing OpenSUSE — but a bit too confusing for a person/distrohopper coming from Ubuntu background.

![](https://miro.medium.com/max/1544/1*cyNlrthMzTj_5RahIUOe2g.jpeg)

Pressing F2 in Refind in order to get to boot item’s options

Sometimes after installation you have a black screen in front of you, like I had with Deepin. It was probably because of the graphics drivers. Oftentimes in Refind by pressing F2 and then

![](https://miro.medium.com/max/1572/1*_3htbCrsDCbl_KjOeYmfEA.jpeg)

Pressing F2 again and typing ‘nomodeset’ to instruct the kernel to not load video drivers

pressing F2 again in the boot option allows you to put the command **_nomodeset_** after the whole boot command (or replace **_quiet splash_** with it). This lets you boot the OS without display drivers (or something along those lines) and then you can install the drivers in peace and get the OS to work normally.

![](https://miro.medium.com/max/1524/1*ojy2zI_c8iIp17utaWq7Bw.jpeg)

Finding out the **.efi** files I wanted to sweep away from the Refind boot manager

With OpenSUSE, I continued the tradition of cleaning Refind by moving the individual **.efi** files (you can read the file name in the boot manager) into some IGNORE folders inside the bootable options’ directories. It’s easy to move them back in case you cleaned a bit too much.

![](https://miro.medium.com/max/1484/1*maQHNoEshVmbiE6HzPfbDQ.jpeg)

Cleaner boot manager. (And the disk icon badges that were visible in an earlier Refind version)

Once I was told that _“The way you’re booting is actually the_ **_EFI stub loader_** _from the kernel. Not all distros come with it automatically._ **_You need the EFI stub loader_** _for this to work.”_ — Well, so far all of the distros I have tried have included this, so, it is going to be a good time to update this article when I encounter a distro that hasn’t got it.

12\. My current favorite distros
================================

As of **2017–01–01**

1.  **Xubuntu** (needs some tweaking after install but all around great and light)
2.  **Zorin OS (12 Beta)** (great for beginners or people coming from Windows)
3.  **Ubuntu**
4.  **Solus OS** (great for beginners but feels a bit unfinished for me)
5.  **Ubuntu GNOME**
6.  **Lubuntu**
7.  **elementary OS** (great for beginners, or people coming from Mac OS X)
8.  **Manjaro XFCE** (a bit technical but great and light. Krita OpenGL was slow, though)
9.  **Debian**
10.  **Kubuntu**

More about my current favorite distros can be found in [**_this article_**](/@manujarvinen/my-current-favorite-distros-5e6b5a2c685c).

I hope you liked the article :)

Feel free to leave a comment below, I would really appreciate that. Especially on your experiences with boot managers and how you manage them when you want to install a new distro.

If you like more of this kind of geeky articles from digital artists, I really enjoy the [**_ones_**](http://www.davidrevoy.com/categorie13/geek) written by **_David Revoy_**.

By the way, just a random observation, if you use Chrome or Chromium, try [**_this extension_**](https://chrome.google.com/webstore/detail/chromium-wheel-smooth-scr/khpcanbeojalbkpgpmjpdkjnkfcgfkhb?hl=en) and put [**_these settings_**](http://i.imgur.com/KTzcUXx.png) in order to feel like 1000% speed gain in your web browsing experience. Whee!

Also, you can use **Ctrl+L** or **Alt+D** to instantly go to the **address bar** of your internet browser. And also to the **location bar** in many file managers.

And, like you probably noticed, this article includes multiple links to other articles I’ve written. Here’s all of them listed, for easy navigation:

┏  [**_Setting up a multi-boot of 5 Linux distributions_**](/@manujarvinen/setting-up-a-multi-boot-of-5-linux-distributions-ca1fcf8d502)┣━━━ [**_What is Linux?_**](/@manujarvinen/what-is-linux-a80383275724)┣━━━ [**_Installing NVIDIA drivers in Linux_**](/@manujarvinen/installing-nvidia-drivers-in-linux-7454f82ef5b)┣━━━━━━ [**_Setting up Elementary OS after installation_**](/@manujarvinen/setting-up-elementary-os-be438f84a0c0)┣━━━━━━  [**_Installing my favorite tools from the PPAs_**](/@manujarvinen/installing-my-favorite-tools-from-the-ppas-c2a46f6d1468)┣━━━━━━━━━ [**_Useful free applications_**](/@manujarvinen/useful-free-applications-70d24998984)┣━━━ [**_Simple Wacom scripts_**](/@manujarvinen/simple-wacom-scripts-6cd381cb320a)┣━━━ [**_Learning some basic terminal commands_**](/@manujarvinen/learning-some-basic-terminal-commands-d478e7b8ffe4)┣━━━ [**_My method for storing files_**](/@manujarvinen/my-method-for-storing-files-49bf71c6edbd)┣━━━ [**_Downloading and verifying .iso images_**](/@manujarvinen/downloading-and-verifying-iso-images-960f75c4d6be)┣━━━ [**_Making bootable USB sticks_**](/@manujarvinen/making-bootable-usb-sticks-cc60b52354b2)┣━━━ [**_Installing Xubuntu_**](/@manujarvinen/installing-xubuntu-efd84051b3dd)┣━━━━━━  [**_Auto-mounting partitions and Fstab_**](/@manujarvinen/auto-mounting-partitions-and-fstab-7722abf8e0e)┣━━━━━━ [**_Setting up Xubuntu after installation_**](/@manujarvinen/setting-up-xubuntu-after-installation-b824080e1f51)┗━━━ [**_My current favorite distros_**](/@manujarvinen/my-current-favorite-distros-5e6b5a2c685c)

Also, you might be interested in this:  
[**_Having excellent passwords easily_**](/@manujarvinen/having-excellent-passwords-easily-259e2dacd757)

> **Manu Järvinen**  
> [manujarvinen.com](http://www.manujarvinen.com)  
> [@manujarvinen](http://www.twitter.com/manujarvinen)
> 
> Thank you:  
> [Jeffrey Hoover](http://italic-anim.blogspot.fi/)
> 
> 2016

[**_Setting up a multi-boot of 5 Linux distributions_**](/@manujarvinen/setting-up-a-multi-boot-of-5-linux-distributions-ca1fcf8d502)” by [**_Manu Järvinen_**](http://www.manujarvinen.com/articles) is licensed under [**_CC BY 4.0_**](https://creativecommons.org/licenses/by/4.0/)

Written by

Manu Järvinen
-------------


#### Animator, 3D modeler and illustrator. Likes open-source stuff like Blender, Linux, Gimp & Krita. And Demoscene. Support me on [Patreon.com/manujarvinen](http://Patreon.com/manujarvinen)

Follow

#### 935

#### 18

#### 

[Some rights reserved](http://creativecommons.org/licenses/by/4.0/)