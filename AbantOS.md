---
title: "AbantOS Linux Users Guide (Elive/Debian)"
author: [See Credits]
date: 2017-11-05
subject: "CSCI 2461-70/2482-40 Fall 2017 Final Project"
tags: [AbantOS, Elive, Linux, Incident Handling, Final Project]
titlepage: true
titlpage-color: "D8DE2C"
titlepage-text-color: "5F5F5F"
abstract: "AbantOS Linux, based on Elive/Debian with the Englightenment Window Manager/GUI. Created by and in support of the Fall 2017 Saint Paul College CSCI 2461-70 Computer Networking 3 - Linux, and 2482-40 Incident Handling & Disaster Recovery classes. "
...

# Collaborative 2461-70/2482-40 Fall 2017 Final Project

## Back Story: Build Your Own Linux

During Week 10 of CSCI 2461-70 and CSCI 2482-40, after significant discussion all students enrolled in these classes unanimiously decided to join efforts and chose as a Collaborative Class Final Project **building their own Specialized Linux distribution** based on Debian Linux and the Elive project. CSCI 2461-70 is focusing on educational components of Linux, and CSCI 2482-40 is focusing on Incident Handling and Disaster Recovery components.   

This document serves as a Users Guide to the 2482-40 and 2461-70 classes and associated tools, techniques and processes. 

## Project Name: AbantOS Linux

In the African Bantu group Abantos means "Belongs to Human Beings." 

AbantOS Linux is based on and created in collaboration with Thanatermesis of the eLiveCD Project. 

The name AbantOS is partially an homage to Ubuntu Linux.  

## Documentation Generation

This document uses the [Eisvogel.tex Template](https://github.com/Wandmalfarbe/pandoc-latex-template) and Pandoc, using a similar method to [OWASP's Top 10](https://github.com/OWASP/Top10/tree/master/2017) [method](https://github.com/OWASP/Top10/blob/master/2017/generate_document.sh)

~~~shell
$ pandoc AbantOS-Elive-UsersGuide-17319-835.md -o ./AbantOS-Elive-UsersGuide-17319-835.pdf --from markdown+multiline_tables --template eisvogel.tex --listings --toc
~~~

## Copyright & License


This work is licensed [Attribution-ShareAlike 4.0 International (CC BY-SA 4.0)](https://creativecommons.org/licenses/by-sa/4.0/).

All contributors hold their original Copyright 2017.

![Attribution-ShareAlike 4.0 International (CC BY-SA 4.0)](AbantOS-Elive-UsersGuide-17319-CC-BY-SA-v4-Intl-88x31.png)



# Introduction

## Social Dynamics of Debian

* The key points to remember in considering Debianos social dynamics as applied to becoming a package maintainer are these:

    - Debian is a volunteer-powered project, and self-motivation and friendly cooperation are crucial.
    - Debianos development is consistently evolving, and maintainers must adapt along with it. This adaptation must be self-driven; you canot rely on others to bring you along.

## Programs Needed for Development

Though many if not all packages necessary for development should come installed with the base Debian installation, itos good to check they are present with *aptitude show package* or *dpkg -s package*. The most important package to make sure is installed is *build-essential*. It should pull in other packages that are necessary for the build environment if theyore not already installed.

Other packages are some measure of helpful and/or necessary for building particular packages down the road. Some are particular compilers for specific languages (i.e. *gfortran* or *gpc*) or packages for scripting languages themselves (i.e. *perl* and *python*). Others make parts of the process easier (such as the *debhelper/dh-make* packages or the *fakeroot* tool) or allow checking for errors after the build (*lintian*). The full range of additional build packages listed should be installed and understood before proceeding.

## Documentation needed for development

This section of chapter 1 includes the debian-policy which is the explanations of the structures and contents of the Debian archive and several OS design issues. It also includes requirements that each package must satisfy to be included in the distribution.  

Important documentations that needed to be read for the development are auto tutorial and gnu-standards.  If any errors or bugs occur, the maint-guide package can be used to report using report bug.

## Where to ask for help?

There are a few documentations that will help solve any problems we encounter during installing packages or using any commands. Available sources to solve any issues are; websites such as lists.debian.org or wiki.debian.org/Teams.

# Debian Package Building 

## Workflow

    1. Get a copy of the upstream software, usually in a compressed tar format.
        * package-version.tar.gz
    2. Add Debian-specific packaging medications to the upstream program under the Debian directory, and create a non-native source package.

        * package_version.orig.tar.gz
        * package_version-revision.debian.tar.gz
        * package_version-revision.dsc

    3. Build Debian binary packages, which are ordinary installable package files in .deb format. The character separating package and version change from hyphen to underscore. As the following command.

        * package_version-revision_arch.deb

## Import Upstream


    1. First, create the first version of a package, outside of Git. Once it done, you can import the package using the import-dsc command.

~~~shell
    $ gbp import-dsc /path/to/package_0.1-1.dsc
~~~

    2. This will give output of its progress and make a few commits as well as have some new files and directories.

~~~shell
    $ ls
    package/
    package_0.1-1.orig.tar.gz
~~~~

    3. git-buildpackage command

    * Imported the package into the upstream branch
    * This has then been tagged upstream/0.1, which represent package's number
    * Imported debian/directory into master and upstream's files
    * Tagged the last commit as debian/0.1-1 as it finished package.

When there is a new version upstream, you could use the following steps to update to the latest version of upstream version.

    $ gbp import-orig -uscan
    $ gbp import-orig /path/to/new-upstream.tar.gz -u 0.2

*if the upstream tarball is already exist in the form of packagename_version.orgi.tar.gz, then the -u option is not required.


## Choosing your program

The steps to follow after choosing a package to create and in order to check if the package is already in the distribution archive are; the aptitude command, the debian packages web page and the debian package tracker web page.  Debian already has packages in the Debian archive which is much larger than that of contributors with upload rights.  If you are able to adopt the package, get the sources something like apt-get source but if you are installing new packages in Debian follow the steps below:

    1. First step is to confirm that the program works. 
    2. Second, Make sure no one is working on the package. If no one is working on it, file an ITP bug report to the wnpp pseudo-package using report bug. If someone else is already working on it, contact them if you feel you need to. 
    3. Third, the software must have a license. 
    4. Fourth, the program should not introduce security and maintenance concerns into the Debian system.  The program should be well documented, should not run setuid root and should not be a daemon.

As a new maintainer, you are encouraged to create simple packages, intermediate complexity packages and high complexity packages. Packaging a high complexity packages requires more knowledge than it takes to build a simple or intermediate packages. 

## Get the program, and try it out.

There are a few steps mentioned on this section to get the package and try it out. 

    1. First step is to find and download the original source code. 
    2. Second, if your package program comes in tar+gzip format with the extension .tar.gz, format with the extension .tar .bz2 or tar + xz format , it usually contain a directory called package-version with all the sources inside. 
    3. Third, if your program comes as some other sort of archive, you should unpack it with the appropriate tools and repack it. 

## Simple build systems

    1. Simple programs comes with a Makefile which can be compiled by invoking make. 
    2. Compile and run to make sure your program works so it wonot break something else while its installing or running. 
    3. You can run make clean to clean up the build directory. 
    4. You can use the make uninstall to remove all the installed files.

## Popular Portable Build Systems

Free software is often written in C or C++, and these often use portable build systems to make them portable across platforms. *Autotools* is one example of such a build system; it comprises *Autoconf, Automake, Libtool, and Gettext*. (Cmake is an alternative build system.)

With these build tools, they must be used to generate the Makefile and other required source files first. The program can then be built using *make; make install*.

The Autotools workflow generally proceeds as:

    1. Upstream runs autoreconf -i -f in the source directory and distributes the generated files along with the source.
    2. The user obtains the distributed source and runs ./configure && make in the source directory to compile the program in an executable source binary.

## Creating a Repository 
Two types of 
dpkg-scanpackages
dpkg-scansource
dpkg-scanpakages arguments | gzip-9c>Packages.gz
apt-get update
apt-get upgrade 
1. open command line 
2. enter following commands 
@repository :+1 :shipit

$ git init
$ git add
$ git commit -m "First commit"
$git remote add origin
$git remote -v 
$ git push origin master 

## Creating a Package 
$ dpkg-buildpackage -us -uc
$ debian/rules clean 
$ debian/rules build
$ fakeroot debian/rules binary

$ apt-get install build-essential dh-make devscripts fakeroot
$ apt-get install patch diff patchutils
$apt-get install linda 



## Source Packages for EFL 
-[] efl-dbp
-[] efl-doc
-[] libecore-dev

## Install EFL 
#create a directory
$mkdir enlight
$cd enlight
#make sure all Dependencies are installed
$ sudo apt install
##EFL build script 

set -e
# Target directory
PREFIX="/usr/local"
 
# List of the needed packages
# To adapt to your needs
PROJECTS="efl enlightenment"
 
# Download url
SITE=" https://git.enlightenment.org/core/"
OPT="--prefix=$PREFIX"
 
PKG_CONFIG_PATH="$PREFIX/lib/pkgconfig:$PKG_CONFIG_PATH"
PATH="$PREFIX/bin:$PATH"
LD_LIBRARY_PATH="$PREFIX/lib:$LD_LIBRARY_PATH"
LOG="installe.log"
rm -f $LOG      # Delete precedent log file
touch $LOG      # Create a log file
date >> $LOG    # Add current date
 
# Download and compile each module
for PROJ in $PROJECTS; do
    # Cloning
    if [ ! -d $PROJ ]; then
        git clone $SITE$PROJ.git $PROJ
    fi
    # Go building and installing
    cd $PROJ*
    make clean distclean || true
    ./autogen.sh $OPT
    make
    sudo make install
    cd ..
    sudo ldconfig
    echo $PROJ" is installed" >> $LOG
done
 
#Optionnal Terminology
git clone https://git.enlightenment.org/apps/terminology.git
cd terminology
./autogen.sh $OPT
make
sudo make all install
cd ..
sudo ldconfig
## Run-time requirements
apt install dbus-x11 xinit xorg








## Linux Distributions

### Ubuntu and Debian: How is Ubuntu related to Linux and Debian?

Ubuntu is a Linux distro that starts with the breadth of Debian and adds regular releases (every six months) commitment to security updated with nine months of support for every release. 

Ubuntu builds on the foundations of Debian's architecture and infrastructure but there are important differences such as, Ubuntu has its own user interface, a separate developer community and different release process. 
Debian is the rock upon which Ubuntu is built. It is a volunteer project that has developed and maintained a GNV/Linux Operating System of the same name over a decade.
Since its launch, the Debian project has grown to comprise more than 1,000 members with official developers status, alongside many more volunteers and contributors.
Debian is a free operating system. An operating system is the set of basic programs and utilities that make your computer run. At the core of operating system is a Kernel. The Kernel is the most fundamental program on the computer and does all the basic housekeeping and lets you start other programs. Debian system use Linux Kernel or the BSD Kernel. Today Debian encompasses over 20,000 packages of free open source applications and documentation.

**About Ubuntu**

Ubuntu is an Open-source project that develops and maintains a cross-platform, open-source operating system based on Debian. Upgrades are released every six months and support is granted by Canonical for up to five years. Canonical also provides commercial support for Ubuntu deployments across the desktop, the server and the cloud.

**How does Debian and Ubuntu fit together?** 

Ubuntu is proud to be based on Debian. Ubuntu developers want to maintain a healthy and collaborative relationship with Debian developers. Ubuntu has brought new users and developers to the GNU/Linux community

**How does Ubuntu differ from Debian?**

Ubuntu has a narrow focus--community based while Debian is Universal.

Ubuntu has its own community government structure somehow inspired by Debian's but different. (MOTU) The masters of the universe were team formed to look after the creation of the universe component.

**Debian for Ubuntu developers**

Ubuntu benefits from a strong Debian and Debian benefits from a strong Ubuntu.

Every Debian developer is also an Ubuntu developer because one way to contribute is to contribute to Debian 
Ubuntu is Debian based.

*Edubuntu*

A complete Linux based operating System targeted for primary and secondary education. It is freely available with community based support.
The Edubuntu community is built on the ideas enshrined in the Edubuntu manifesto: that software, especially for education should be available free of charge and that software tools should be usable by people in their local language and despite any disabilities. [14]

*Kubuntu*

An official derivative of Ubuntu Linux using ICDE instead of the GNOME or unity interfaces used by default in Ubuntu.

### RedHat/Fedora Based

### ArchLinux Based

## Linux Window Managers

### GNOME

GNOME (GNU Network Object Model Environment, pronounced as NOHM) is a graphical user interface (GUI) and set of computer Desktop applications for users of the Linux computer operating system. Its intended to make a Linux operating system easy to use for non-programmers and generally corresponds to the windows desktop interface and its most common applications. (Wikipedia)

### Englightnment 

**AbantOS** uses the Enlightenment Window Manager

## Elive background/history
	
## Elive purpose/objectives

## Teaching Linux Objectives

## Teaching  Linux Scope

## Finding Help

```shell
>>> help
```

# Installation of Elive

## Downloading Elive from [EliveCD.org](www.elivecd.org)

* The latest release of Elive can be downloaded from www.elivecd.org. The latest Beta release is the recommended version to download.
* Navigate to the Beta downloads by clicking Download, then Beta.
* For writing Elive to a USB drive, click the button labeled Elive Beta - USB. This is an Elive image specifically for writing to USBs. A window will open asking if you want to open or save the .img file. Click Save File and your browser will download the file.
* When the download finishes, open the folder to which it was saved and take note of the file's location.

## Release notes at [EliveCD.org](http://www.elivecd.org/news/releases/beta/)

The archive of release notes on the Beta releases of Elive can be found at http://www.elivecd.org/news/releases/beta/. Each entry contains many details on what features of Elive have been updated and improved in each release.

## Writing Elive to USB

* The recommended USB drive for writing and using Elive is the SanDisk Extreme USB 3.0 32 GB flash drive.
* If you are using Windows, you can write the Elive image to the USB using an image writer application. Elive suggests UsbWriter; other options include the application from Etcher.io or Win32DiskImager.
* With any application, first plug your USB drive into an open USB port on your computer. Using the application, select the Elive image from the location you downloaded it to, then select the correct USB drive and write the image.
* The written image will overwrite any other data contained on the USB drive. In some cases, the USB drive may need to be reformatted first before being able to write a new USB image (for instance, if a previous USB image was written to the drive). You can reformat the drive by right-clicking the drive in the Computer window or using Windows Disk Management.

## Booting Elive from the USB

* Power on your computer with the USB with Elive written to it plugged in. When the initial boot screen of your computer appears, hold down the Boot Menu key to load the boot menu. On many computers this is the F12 key, or one of the other F row keys. A list of common boot menu keys for various manufacturers can be found at http://www.elivecd.org/ufaqs/how-to-tell-the-computer-to-boot-from-another-device/.
* When the boot menu key appears, highlight the USB drive option and press Enter.
* After Elive initiates, the first Elive menu asking the method of installation will appear. For running Elive from the USB, select Live sessions with Persistence. This method will allow you to run Elive direct from the USB without installing the system on your separate hardware, and Persistence will save your configurations of Elive from session to session.

### What is BIOS?
First and foremost BIOS is pronounced by-oss and is sometimes referred to as the System BIOS, ROM BIOS, or PC BIOS
The word BIOS stands for Basic Input Output System. It is a software that can be located on a memory chip on the motherboard. The BIOS gives the computer instructions to perform some  functions like booting and keyboard control and also configure the hardware in a computer including the hard drive, floppy drive, USB, optical drive, CPU, memory to list a few, it also help in the proccess of trouble shouting. The BIOS is also responsible for the POST (POST stands for "Power On Self Test." It is a diagnostic program built into the computer's hardware to tests different hardware components before the computer boots up. The POST process is run on both Windows and Macintosh computers). reason why it the first software to run when a computer is started (BIOS). The BIOS firmware settings are always saved and recoverable even after power has been removed from the device.
The BIOS is can be accessed and configured through the it's Setup Utility and is pre-installed before the computer is purchased. Unlike the operating system such as Windows, which needs to be  downloaded or obtained on a disc, and needs to be installed by the user or the manufacturer. 

  ### EFI and UEFI
Standing for Extensible Firmware Interface, EFI is a recent firmware developed by Intel and introduced with the release of IA-64. EFI's version has improved the fonctions previously available in the BIOS. The EFI specification was made into a general standard known as UEFI. The EFI System Partition (ESP) is the partition on a data storage device (usually a hard disk drive or solid-state drive) that is used by computers to the Unified Extensible Firmware Interface (UEFI). When a computer is booted (also meaning started), UEFI firmware loads files stored on the ESP to start the installation of the operating systems and other utilities. The ESP needs to be formatted with a file system whose specification is based on the FAT file system and maintained as part of the UEFI specification; so the file system specification can be independent from the original FAT specification. EFI can be used to eliminate the boot loader, that will allow the firmware to select the operating system and at the same time enables vendors to create drivers that cannot be reversed or engineered to contains a small shell. This process can be run at boot and allow a small and manageable working environment without compromising anything on the computer. It has been noticed nowadays that new computers use UEFI firmware instead of the traditional BIOS. Both are still low-level software that starts at the same time as the computer is started at the boot level, but UEFI is a more of a modern solution. It was devellopped in the intention to make the BIOS better. reason why it supports larger hard drives, make the boot proccess go faster, has more security features, and-conveniently-graphics and mouse cursors. There are newer PCs that has the chip with UEFI still refer to as the “BIOS” to avoid confusing people who are used to a traditional PC BIOS, but does not mean that the chip still has the BIOS running. Even if your PC uses the term “BIOS”, modern PCs baught nowadays almost al of them has certainly ship with UEFI firmware instead of the old BIOS.

 * Why the BIOS Is Outdated
To be clear BIOS has not been labeled bad or wrong, all this new developemnt are only for improuvment. The BIOS has been around for quite a long time, and was not evolving much. Even MS-DOS PCs released in the 1980s still had a BIOS on the ship on the motherboard. But still modification have been made to improve the BIOS along all those years. Some extensions were developed, all in the name of improvment such as ACPI (the Advanced Configuration and Power Interface). This allows the BIOS to more easily configure devices and perform advanced power management functions, like sleep which is not off. But the BIOS hasn’t advanced and improved nearly as much as other PC technology has since the days of MS-DOS. Still The BIOS was experiencing some limitations. For example tt can only boot from drives of 2.1 TB or less, while 3 TB drives are common now, and a computer with a BIOS can’t boot from them. That limitation is due to the way the BIOS’s Master Boot Record system works. The BIOS must run in 16-bit processor mode, and only has 1 MB of space to execute in. It has trouble initializing multiple hardware devices at once, which leads to a slower boot process when initializing all the hardware interfaces and devices on a modern PC. The BIOS was basically crying for replacement or improvment if you want to call it for a very long time. Intel started working on the Extensible Firmware Interface (EFI) specification back in 1998. Apple chose EFI when it switched to the Intel architecture on its Macs in 2006, but other PC manufacturers didn’t follow. In 2007, Intel, AMD, Microsoft, and PC manufacturers agreed on a new Unified Extensible Firmware Interface (UEFI) specification. This is an industry-wide standard managed by the Unified Extended Firmware Interface Forum, and isn’t solely driven by Intel. UEFI support was introduced to Windows with Windows Vista Service Pack 1 and Windows 7. The vast majority of computers you can buy today now use UEFI rather than a traditional BIOS.

   ### Master boot record
The Master Boot Record (MBR) is a set information located in the first sector of hard disks or diskette that identifies. It has the prepuse of detecting how and where an operating system is located so that it can be boot (loaded) into the computer's main storage or random access memory. The Master Boot Record is also refer to as the "partition sector" or the "master partition table" because it includes a table that locates each partition that the hard disk has been formatted into. The MBR also includes a program that reads the boot sector record of the partition that contains the operating system to be booted into RAM, that record contains a program that loads the rest of the operating system into RAM. The MBR, is the most important data structure on the disk and it is created when the disk is partitioned. it contains a low amount of executable code refer to as the master boot code, the disk signature, and the partition table for the disk. At the end of the MBR there is a 2-byte structure called a signature word or end of sector marker. This sector is always set to 0x55AA. A signature word also represent the end of an extended boot record (EBR) and the boot sector.
A disk signature is known to be a unique number at offset 0x01B8, that set out the disk to the operating system. Windows 2000 for example uses the disk signature like an index to save and retrieve information about the disk in the registry.
 * Master Boot Code
The master boot code runs numerous features like, Scaning the partition table for the active partition; Finding the starting sector of the active partition, Loading a copy of the boot sector from the active partition into memory, Transfering control to the executable code in the boot sector. In case the master boot code is unable to complete any of these activities, the system will be displaying one an error messages like; Invalid partition table; or Error loading operating system; or even Missing operating system
but there is to say There is no MBR on a floppy disk. The first sector on a floppy disk is the boot sector. Every hard disk contains an MBR, the master boot code is used only if the disk contains the active, primary partition.

 ### GUID partition type
* What is a GUID Partition Table disk?
The GUID Partition Table disk architecture was introduced as part of the Extensible Firmware Interface initiative. GUID Partition Table is a new disk architecture that expands on the older Master Boot Record (MBR) partitioning scheme that has been common to Intel-based computers. A partition is a contiguous space of storage on a physical or logical disk that functions as though it were a physically separate disk. Partitions are visible to the system firmware and the installed operating systems. Access to a partition is controlled by the system firmware and the operating system that is currently active.
* Why do we need GUID Partition Table?
GUID Partition Table disks can grow to a very large size. In July 2001, the Microsoft implementation supports a hard disk of up to 18 EB (512 KB LBAs). The number of partitions on a GUID Partition Table disk is not constrained by temporary schemes such as container partitions as defined by the MBR Extended Boot Record. The Microsoft implementation of GUID Partition Table is limited to 128 partitions. However, it is important to note that one partition is used for the EFI System Partition, one for the Microsoft Reserved and two more are used if you use dynamic disks. This leaves 124 partitions for data use. The GUID Partition Table disk partition format is well defined and fully self-identifying. Data that is critical to the operating system is located in partitions and not in partitioned or "hidden" sectors. GUID Partition Table does not allow for hidden sectors or partitions. GUID Partition Table disks use primary and backup partition tables for redundancy and CRC32 fields for improved partition data structure integrity. The GUID Partition Table partition format uses version number and size fields for future expansion.Each GUID Partition Table partition has a unique identification GUID and a partition content type, so no coordination is necessary to prevent partition identifier collision. Each GUID Partition Table partition has a 36-character Unicode name, which means that any software can present an easily readable name for the partition without any additional understanding of the partition.

### Booting (Mac) from an external USB device

**References**
"[How to start up Mac from bootable media](http://www.idownloadblog.com/2015/09/14/how-to-start-up-mac-from-bootable-media/)"

### Booting (PC) from an external USB device

### USB Booting Issues

## Installing on hardware (or Live sessions w/ Persistence)

## Help with Installation

## Keeping Elive Up to Date

```shell    
>>> apug

```

```shell
>>> sudo apt-get dist-upgrade

```

# Graphics and Themes

Elive utilizes the Enlightenment Window Manager. Enlightenment utilizes a set of core libraries called (EFL), created specifically for Enlightenment. EFL is being recognized for its forward thinking approaches as users are wanting more from their operating systems UI. 

## Elive Tools

[elive-tools](https://github.com/Elive/elive-tools) are a set of handy and useful tools by Elive, for specialty use in the [Elive project]()

* audio-configurator
    - configure individual alsa cards
* bkp (save states)
* elivepaste (pastebin)
* waitfor
    - waits on the shell for a process that terminates and exits, its pretty useful for do things like: $ waitfor rebuild-packages && rebuild-iso
* user-manager
* make-cookies-with-chocolate


## Enlightenment Foundation Linux (EFL) Components

1. **Evas** 
    - Core scene graph and rendering
2. **Elina** 
    - Data strucures and low level helpers
3. **Edje** 
    - UI layout & animation data files for themes
4. **Eet** 
    - Data (de)serialization and storage
5. **Ecore** 
    - Core loop and system abstractions like x11
6. **Efreet** 
    - Freedesktop.org standards handling
7. **Eldbus** 
    - D-Bus glue and handling 
8. **Embryo** 
    - Tiny VM and compiler based on pawn
9. **Eeze** 
    - Device enumeration and access library
10. **Emotion** 
    - Video decode wrapping, glue and abstraction
11. **Ethumb** 
    - Thumbnailing handler
12. **Ephysics** 
    - Physics (bullet) wrapper and Evas glue
13. **EIO** 
    - Asynchronus I/O handling
14. **Evas Generic Loaders** 
    - Extra image loaders for complex image types
15. **Emotion Generic Players** 
    - Extra video decoders (for vlc)
16. **Elementary** 
    - Widgets and high level abstractions

## Customizing Your Elive/Enlightenment Theme

Elive currently has three theme options available by default.


1. **Default Theme**
2. **Elive Light Theme** 
    - A light theme
3. **Lucax Theme** 
    - A dark theme

### Change Themes

Right click on the desktop to bring up the menu, hover over Enlightenment to bring up enlightenment sub menu. Hover over about theme and click it, this will show you the current theme.  Close the about theme and right click on the desktop again to bring up the menu. Hover over Settings, scroll down and hover over Theme, you will see the three options available. Right click on the Theme you want and wait for it to change, this may take a minute or two depending on the theme. Open windows and applications will lock up during the transition, this is normal.


### Change Wallpaper

Right click on the desktop to bring up the menu. Hover over Settings and right click it. A settings window will appear. Right click on Wallpaper, the wallpaper settings will appear. Choose one the the wallpapers and click apply. 

### Live / Animated Wallpapers

There are two methods for doing this that I know of, here are both.


#### Live Wallpaper: Method 1


The first method is to download an HD Wallpaper in .gif format. You can download one here: Animated Wallpaper Sample gif. After you finish downloading your animated wallpaper you have to put it in the right directory open a Terminology terminal and navigate to /Downloads, or wherever you have your .gif wallpaper you downloaded. Now you have to move the wallpaper to the /usr/share/enlightenment/data/backgrounds/ . The command to move the file is:

~~~shell
sudo mv "name of file without quotes.gif" /usr/share/enlightenment/data/backgrounds/
~~~

Close the terminal and right click on the desktop to bring up the menu, click settings and then Wallpaper. The wallpaper settings will appear and it will open the directory where you just moved your wallpaper. Choose your animated wallpaper and click apply. Close the wallpaper settings.

NOTE: If you didn't see your wallpaper then your command may have been mistyped, don't forget to remove the quotes.


#### Live Wallpaper: Method 2


The second method is to download a HD wallpaper .gif and transfer it with the Thunar File Manager. Download your gif using the link above or any you choose in .gif format. Open the thunar file manager and from the home directory click downloads, or navigate to your .gif wallpaper. Open another thunar file manager window by clicking the icon in the dock again. Press (control) and (h) at the same time to show hidden files. Click .e and then e17, now click backgrounds. Drag and drop your animated wallpaper .gif from downloads to the backgrounds directory you just opened. After you transfer the wallpaper press (control) and (h) again to hide the config files. Close the file managers

Right click on the desktop to bring up the menu, click settings and then Wallpaper. The wallpaper settings will appear. In the top left click Personal, now you should see the wallpaper you transferred in the list below, click it. Now click apply and close the wallpaper settings.

## Advanced Customizations

### Settings/Look

#### Application Theme

#### Theme

#### Colors

#### Fonts

#### Borders

#### Ecomorph

#### Transitions

#### Scaling

#### Startup

### Settings/Apps

#### Personal Application Launchers

#### Favorite Applications

#### IBar Applications

#### Screen Lock Applications

#### Screen Unlock Applications

#### Restart Applications

#### Startup Applications

#### Default Applications

#### Desktop Environments


### Settings/Screen

#### Virtual Desktops

#### Screen Setup

#### Screen Lock

#### Blanking

#### Backlight


### Settings/Windows

#### Window Display

#### Window Focus

#### Window Geometry

#### Window List Menu

#### Window Remembers

#### Window Process Management

#### Windows Switcher

### Settings/Menus

#### Menu Settings


## Elive Desktop Features

### Main Elive Menus

### Finding installed applications

### Changing appearances/preferences

### Restoring settings


## Elive Desktop Help Features


# Installing/Removing applications

## Installing from the terminal (apt-get)

## Installing with a software package manager (i.e. Synaptic)

## Removing from the terminal

## Removing with a package manager

## Updating system and applications

## Help with Installation and Removal of Applications


# Command references

## Whatever commands the class elects would be useful to include in a quick reference

## Where to get help

## From basic to advanced

# Repository 

## Creating Repositories

## Setup and Maintain 

There are various reasons for building a repository yourself. You might just have a few packages with local modifications that you want to make available to apt. You may want to run a local mirror with those packages used by several machines to save bandwidth, or build packages yourself to test them prior to publication.

## Anatomy of a Repo

A Debian repository contains several releases. Debian releases are named after characters from the "Toy Story" movies (wheezy, jessie, stretch, ...). The codenames have aliases, so called suites (stable, oldstable, testing, unstable). A release is divided in several components. In Debian these are named main, contrib, and non-free and indicate the licensing terms of the software they contain. A release also has packages for various architectures (amd64, i386, mips, powerpc, s390x, ...) as well as sources and architecture independent packages.

* Root Directory = dists 
* Each release subdirectory contains a cryptographically signed Release file and a directory for each component. 

## Repository Index File 

1. dpkg-scanpackages

~~~shell
$ dpkg-scansources dists/local/custom/source | gzip > dists/local/custom/source/Sources.gz
~~~

2. dpkg-scansources
~~~shell
$ dpkg-scanpackages dists/local/custom/binary-i586 | gzip > dists/local/custom/binary-i586/Packages.gz
~~~

## Override File

An override file can be optionally specified. If no override file exists/, dev/null must be provided as an explicit argument. 

## Working with Repos

Working with repositories may mean either of two different things:

1. You can use a repository with the apt family of programs (apt, apt-get, apt-cache, aptitude) to browse or install packages

~~~shell
echo "deb http://ftp.debian.org/debian stable main contrib non-free" >> /etc/apt/sources.list && apt-get update
~~~

2. You can set up a repository yourself and add, remove or replace packages in it.


## Repository References

    wiki.debian.org/DebianRepository
    wiki.debian.org/DebianRepository/Setup


# Systems Administration & Configuration

    1. Boot process and Troubleshooting
    2. System Logging
    3. Disk Management and File System Creation
    4. Managing Linux Users and Groups
    5. Linux Networking (setting up networking and config)
    6. Process and Job Management
    7. Creating, Viewing and Editing Text Files (Vi)
    8. Installing and Updating Software Packages
    9. Scheduling and Automating Future tasks
    10. Network Storage and Management (NFS, SMB)
    11. Service Configuration (DHCP, FTP, DNS, SSH, HTTP etc...)    
    12. Linux Shell Scripting

## User Management

To maintain the privacy of users in the Linux system, they have managed their accounts and supervision as well as tracking the problems they will go through to find the solutions to these problems. They will inform users of the changes and updates. Even if there are one or more user using the system it will always be a challenge for the managing users. I will show you some tools and some tasks assigned to the user in the Linux system where you will see adding and deleting accounts.
Before we talk about how to create and delete a user account, I will talk about the most important text file(/etc/passwd) because its a important process to the information about users in the Linux system.

This file it is open to read only for users, and it is read and writable for root account.

### /etc/passwd file

The /etc/passwd file contains only one separate line, limited by a colon (:) for each user account in the system, also it is stored the information for each user using the Linux system. Every time we add new user to the system all details and information for the new user will stored in the same file. 

root /h/jamal  >>> sudo  cat  /etc/passwd

jamal: x:1001:1001: jamal Ibrahim, 1,,:/home/jamal :/bin/bash

tyler: x:1002:1002: tyler, 1,,:/home/tyler :/bin/bash

awil: x:1003:1003: awil, 1,,:/home/awil :/bin/bash

jacquie: x:1004:1004: jacquie, 1,,:/home/jacquie :/bin/bash

matthew: x:1005:1005: matthew, 1,,:/home/matthew :/bin/bash

to get depth in the file I will take the entry of my user name jamal.

 

Jamal       :    x      :   1001 :   1001   :    jamal Ibrahim   ,    1   ,,   :   /home/jamal    :   /bin/bash

{---1----}{---2---}{---3-----}{---4----}{---------------5---------------}{----------6------}{-------7-----}

### /etc/passwd fields

    1. **Username field**: This field show the user account name; this user name must be used every time when we log in to the system.
    2. **Password field**: Second field is the Password field, not showing the actual password though. The 'x' in this field show the password is encrypted and saved in the /etc/shadow file.
    3. **UID field**: Every time when a user account is created, it is assigned with a user id or UID (UID for the user 'jamal' is 1001, in this case) and this field specifies the same.
    4. **GID field**: Similar to the UID field, this field specifies which group the user belongs to, the group details being present in /etc/group file.
    5. **Comment/Description/User Info field**: This field is the short comment/description/information of the user account.
    6. **User Home Directory**: Every time the user sign in to the system, he is taken to his Home directory, where all his personal files reside. This field provides the absolute path to the user's home directory (/home/jamal in this case).
    7. **Shell**: This field show, the user has access to the shell mentioned in this field (user 'jamal' has been given access to /bin/bash or simply bash shell).

## Adding new user accounts to Linux system

# Package Management

Elive is based on Debian, the same distribution flavor as Ubuntu, so we use .deb files for our packages and those files collectively live in a repository.

## Debian Package Management

Debian Package Management details can be found in the [Debian Manuals](https://www.debian.org/doc/manuals/debian-reference/ch02.en.html)

--------------------------------------------------------------------------------
package             popcon           size       description
---------           --------         ------     -------------
apt                 V:871, I:999     3647       Advanced Packaging Tool (APT), front-end for dpkg providing "http", "ftp", and "file" archive access methods (apt-get/apt-cache commands included)

aptitude            V:136, I:778     4415       interactive terminal-based package manager with aptitude(8)

tasksel             V:40, I:972      374        tool for selecting tasks for installation on the Debian system (front-end for APT)

unattended-upgrades V:188, I:364     254        enhancement package for APT to enable automatic installation of security upgrades

dselect             V:4, I:59        2498       terminal-based package manager (previous standard, front-end for APT and other old access methods)

dpkg                V:932, I:999     6745       package management system for Debian

synaptic            V:70, I:453      7793       graphical package manager (GNOME front-end for APT)

apt-utils           V:391, I:997     1103       APT utility programs: 

apt-listchanges     V:350, I:838     365        package change history notification tool

apt-listbugs        V:7, I:12        451        lists critical bugs before each APT installation

apt-file            V:13, I:77       82         APT package searching utility -- command-line interface

apt-rdepends        V:0, I:6         40         recursively lists package dependencies
--------------------------------------------------------------------------------
Table: Debian package management (Manual), List of Debian package management tools

Source: [Debian package management manual](https://www.debian.org/doc/manuals/debian-reference/ch02.en.html) and [Popularity Contest Stats](https://qa.debian.org/popcon.php?package=apt)

## Elive Packages

In order to understand the components of the Elive repository here are a couple commands

### Listing Packages

```shell

for package in $( dpkg -l | awk '{print $2}' ) ; 
    do if apt-cache policy "$package" 2>/dev/null | grep -qs "repository.elivecd.org" ; 
    then echo -e "${package%:*} \tis in Elive repos" ; 
    fi ;  
done

```

### Listing Package Count

```shell

cat /var/lib/apt/lists/repository.elivecd.org_*Packages | grep "^Package:" | wc -l

```

## Installing Packages 

```shell

sudo apt-get

```

1. acptool: replacement of apmtool. For laptop users.

2.  afflib- dpg- on-disk format for storing complete forensic information: sudo apt-get install afflib-dbg, allows files to be digitally signed, chain-of-custody and long-term file integrity. Allows disk images containing privacy sensitive material to be stored on the Internet. Posix stricpting (Linux coding).

3. balsa-dbg: get sudo apt-get install balsa-dbg. Mail client from GNOME desktop. Supports both POP3 and IMAP.

4. cython3-dbg: sudo apt-get install cython-dbg. Libraries built against versions of Python configured with pydebug. Allows python configuration.

5. /etcldibbler/client.conf w/new version. DHCPv6 clients: sudo apt-get install dibbler-client

6. sudo apt-get install DVd+rw-tools-dbp

7. sudo apt-get update

8. sudo apt-get install build-essential save files and execute.

9. sudo apt-get install enigma 


# Testers & Quality Assurance

## What are some patterns we should test? 

## What steps should Testers & QA go through? 


# User Case Studies

## How are we going to use this? 

## What are your biggest challenges to learning Linux? 

## What tools and techniques are helpful to you? 

Talk to potential users!

# Credits

## Contributors

### Jacquie Kerongo

Project Lead and Troubleshooting
How is Ubuntu related to Linux and Debian?

### Angela Griffin

Package Management

### Jamal Ibrahim 

User Management

### Mohamed Mohamud

Repository & Systems Administration

### Tyler Cobb

Scribe
Quality Assurance / Testing

### Rodas Mekonnen

Cat Hearding
Editing

### Roxanne Thao

Package Management

### Doung Doth

USB Booting (Mac)

### Blia Vue

QA: Debian Package Management Workflow

## Editors

### Kautz, D.E.

Project Editor
Original Draft (2017-10-30)
Installation of Elive

### Harmon, M.J.

Formatting
