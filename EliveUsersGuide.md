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

During Week 10 of CSCI 2461-70 and CSCI 2482-40, after significant discussion all students enrolled in these classes unanimiously decided to join efforts and chose as a Collaborative Class Final Project **building their own Specialized Linux distribution** based on Debian Linux and the Elive project. CSI 2461-70 is focusing on educational components of Linux, and CSCI 2482-40 is focusing on Incident Handling and Disaster Recovery components.   

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

First things first. BIOS stands for Basic Input Output System referred to as BIOS, is software stored on a small memory chip on the motherboard. It give the computer a set of instructions on how to perform some of the basic functions such as booting and keyboard control as well as configure the hardware in a computer including the hard drive, floppy drive, USB, optical drive, CPU, memory to list a few.

### What is EFI and UEFI? 

### What is a Master Boot Record?

### What is a GUID partition type? 

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

**cat _files_** - Prints files to screen.

**cd _directory_** - Change to particular directory.

**pwd** - Prints the current working directory.

**cp _files_ _dest_** - Copies files and directories.

**echo _string_** - Echoes a string to screen.

**ls _files_** - List files.

**mkdir _directory-name_** - Create directories.

**mv _file1 _file2_** - Move/rename files.

**rm _files_** - Remove files.

**grep _searchstring_ _files_** - Find a particular search-string in files.

**ps _options_** - Show current processes.

**kill _[-9]_ _PID_** - Send signal to terminate a particular process (process ID).

**su - _[username]_** - Switch user (e.g., become root).

**sudo _commannd_** - Execute a partcular command as root.

**dpkg -l _names_** - List packages.

**dpkg -I _pkg_.deb** - Show package information.

**dpkg -c _pkg_.deb** - List contents of package file.

**touch _file_** - Create an empty file.

**less _file_** - Display file content one screenful at at time.

**ln -s _target_ _linkname_** - Create a symbolic link from a target to linkname.

**lsof** - List open files and processes using them.

**find _directory_ -name _file_ -print** - Find a file location in a directory.

**gunzip _file_.gz** or **gzip _file_** - Uncompress or compress a .gz file.

**tar cvf _archive_.tar _file1_ _file2_ ...** - Create a compressed archive containing multiple files.

**tar xvf _archive_.tar** - Unpack a .tar file.

## Whatever (other) commands the class elects would be useful to include in a quick reference

## Where to get help

**man _page_ or man** - Read online help for a particular, or every, command.

**_command_ --help** - Brief help for a particular command.

## From basic to advanced

# Repository 

## Creating Repositories

## Setup and Maintain 

There are various reasons for building a repository yourself. You might just have a few packages with local modifications that you want to make available to apt, you may want to run a local mirror with those packages used by several machines to save bandwidth, or you build packages yourself and want to test them prior to publication.

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



# Elive Linux Basic System Administration


### By Mohamed Mohamud and Mohamed Mohamed

## Topic Areas

* Boot Process and Troubleshooting.
* Basic System Configuration.
* System Logging.
* Disk Management and File System Creation.
* Linux Networking and Configuration.
* User and Group Management
* Process and Job Management.
* Creating and Editing Text Files.
* Installing and Updating Software Packages.
* Scheduling and Automating Future Tasks.
* Service/Server Configuration (DHCP, FTP, DNS, SSH and HTTP).

#### Section 1.1 Boot Process

When you first boot an Elive Linux Operating System or any Linux OS it will go through the following processes to boot into the command line or desktop. When you press the power button on a machine that is running Elive, the first program that runs in the BIOS. BIOS stands for Basic Input Output System. We must clarify about the BIOS before we go any further.  To be clear the BIOS is not part of the Linux kernel. The BIOS is separate of the operating system, it is not part of the OS. It can be found on computers running other operating systems such as Windows and others. The BIOS is a software that is built into a computer's ROM (Read Only Memory) at the time of manufacturing the motherboard. The BIOS goes through two main stages to boot a Linux kernel. It first does POST (Power On Self Test). In this first stage the BIOS checks to ensure that peripheral devices are intact and ready to operate the system after boot up. Peripheral devices are things like the keyboard, monitor, mouse, RAM, CPU and so on. Below is a screenshot of a BIOS.

![bios](https://user-images.githubusercontent.com/26585912/33965743-b410efb4-e022-11e7-998d-6ea313aadbe0.PNG)

*Figure 1.1 BIOS.*


The next stage of the BIOS is that it looks for an MBR (Master Boot Record) on all the drives that are connected to the system. The BIOS has a boot menu which lists the boot order of all bootable devices that are available on a given system. Most BIOS by default are configured to check for the MBR on the first disk drive onboard. If there is no disk drive it'll check for a CD/DVD ROM or USB; which basically means it'll go through all the drives that are connected or onboard until it finds a drive with an MBR. Then it hands of the boot process to the drive that has the MBR. Another screenshot of the BIOS's boot menu.


![bios2](https://user-images.githubusercontent.com/26585912/33965751-bab72dc4-e022-11e7-9e55-7d56624d57c3.PNG)

*Figure 1.2 BIOS boot menu.*



The MBR is the first sector of a disk drive; it holds a table of partitions on the disk drive. The MBR will look all the partitions on the drive to find one that has a boot loader. There are many common boot loaders but Elive linux uses GRUB two. Below is a screenshot of the GRUB two startup menu that is getting ready to boot up an Elive linux. 

![grub2](https://user-images.githubusercontent.com/26585912/33965774-c7be93e0-e022-11e7-9b6e-3989717e64c3.png)
*Figure 1.3 GRUB Boot Menu.*

#### Section 1.2 Troublshoot Boot Process

Sometimes during the boot process you can into a some issues such as hearing a beep and the system getting stuck on first boot page.  Usually you get an error message that gives you a hint as to what might be the problem or an error that tells you exactly what may the problem. If you hear a beep that is usually in indication that the BIOS is either not detecting a peripheral or it is not able to communicate with it. In this case, follow the error message and figure out what is wrong with that device.  For instance, if you have an error that points to keyboard not detected make sure all the cabling to the PC is properly plugged in and that you don't have any lose connections. BIOS errors are not a kernel issue, that is just physical hardware issues. 

The next type of error you can run into is GRUB not finding an OS, this is a common error, this just means that when the BIOS handed off the boot process to the MBR which found GRUB, that specific disk drive does not have an OS installed on it or the installation is corrupt, the easiest solution is to reinstall the OS. But below are a few commands you can run to detect if you have a corrupt installation or of something else is going on. 

From the GRUB initial Boot Menu, use the up/down arrow keys to interrupt the boot process, then press "e" to enter into the GRUB configuration mode.

I have two linux OSs setup in dual boot but only one of them boots, the other one is not showing up in GRUB's menu. This is what to do when the system first boots. From the GRUB menu, enter "c" to go into grub command line configuration. Below is a screenshot of how that page looks like. Refer to the bottom of Figure 1.3 above to see the options.

![grub2-2](https://user-images.githubusercontent.com/26585912/33965787-d0580d88-e022-11e7-8c7a-6173b153ec52.png)

*Figure 1.4 Grub CLI*

In this menu, some of the available commands are the "ls" command and its options as well. In the above case, it is a simple problem, the second OS which is Debian 7 doesn't have a boot option declared in the grub file. By adding that configuration in grub that will take care of the problem and next time you reboot, GRUB will list multiple OS to boot from; in our case ELive linux and Debian 7. 


The command to change the system's hostname is **hostname** followed by whatever name you want to give it. You must be root to change the hostname or use the "sudo" option before the command. Running the **hostname** command by itself will return the current name of the system. 

```
root ~ >>> hostname
Elive
```
To change it now do the following. 

```  
root ~ >>> hostname Debian
root ~ >>> hostname
Elive
```
Now, we changed the name but when we run the **hostname** command again it returns the old name. The reason is because this change is temporary on this shell. You must start another shell or login again to see the change.  Besides that, most changes on the shell will not survive a system reboot, therefore, If you want to change the system's hostname permanently you must modify the /etc/hostname file.

```
[aali@elive:~]$ sudo nano /etc/hostname
```
![hostname](https://user-images.githubusercontent.com/26585912/33965802-de5fe694-e022-11e7-89dc-b75f741aaf91.PNG)

*Figure 2.1 /etc/hostname*



#### System Logging commands

     a. dmesg (Will list the kernel boot process)

     b. To see the logs for apache go to /var/log/apache2
     
     3. Disk Management and File System

     a. After loading a new drive use "ls -l /dev/sd* or hd etc...

     b. To partiton the drive using fdisk use this command as root "fdisk /dev/sd*"

### Managing Users and Groups

    a. To modify an existing user account use "usermod " command

    b. To delete an existing account an all it's files use "userdel -r"

### Linux Networking

    a. To check the routing table use this command "route"

    b. To add a gateway for an interface use this command "route add default gw x.x.x.x inter_name"

### Processes and Job Management

    a. To kill an existing process use the "kill PID" command

    b. To view processes in a tree like format use "pstree | more"
    
    7.  Creating, Viewing and Editing files

    a. To use vi, use this command "vi fileName"

    b. To go into insert mode hit "i" or "a"

 ### Installing and Updating software packages

    a. If you want to see what packages you have installed use this "dpkg -l"

    b. To remove a package you don't need use "apt-get remove package_name"



 ### Network Storage and Management

    a. To mount an NFS show drive use "mount /dev/sdx /mnt/mount_point_name"

    b. To unmount an NFS drive use "umount /dev/sdx /mnt/mount_point_name"

### Service Configuration

     a. To see the status of a running service use this "service service_name status"

     b. To restart a running service use this "service service_name restart"

### Here is a simple shell script that will print something on the screen.

     vi fileName

     #!/bin/bash

     echo "Hello Linux Class!"

     exit file and run with " ./fileName"
     
    This is our work for week14, We've also created a branch on github and uploaded this file. 

1. Boot process and Troubleshooting
The command to get into grub after boot up is "grub"
And use "quit" to get out of grub

2. System Logging

3. Disk Management and File System Creation

To modify or add a new partition use "parted /dev/sdx" command
To display existing partitions within a disk use "print"

4. Managing Linux Users and Groups

to modify an existing group use "groupmod"
to display a specific user's group and uid use "id username"

5. Linux Networking (setting up networking and config)
"ip addr" is another command that display the info of interfaces
To display network connections use "netstat -r"

6. Process and Job Management

7. Creating, Viewing and Editing Text Files
To quit vi without saving your changes type escape ":q!"
To cut a like in a file using vi, use "y" from edit mode

8. Installing and Updating Software Packages
To compile a source file and install
./configure (from it's config direcotory where the configure file is)
make
make install

9. Scheduling and Automating Future tasks
crontab file is one of the tools used to auto make future tasks. 
to get into the crontab file use this "crontab -e"
to list any pending jobs use "crontab -l"

10. Network Storage and Management (NFS, SMB)

Here is a mount command for nfs
mount -o rw serverName:/dir /mnt/dir

11. Service Configuration (DHCP, FTP, DNS, SSH, HTTP etc...) 
to install ftp and start it use the following commands. 
sudo apt-get install vsftpd
sudo service vsftpd start

12. Linux Shell Scripting
     
   
   ### Command line: File permissions The commands for modifying file permissions and ownership are:

chmod – change permissions

chown – change ownership.

remember the only user that can actually modify the permissions or ownership of a file is either the current owner.
or the root user.

you can change the permissions of the folder to give them access. One way to do this would be to issue the command: 

-sudo chmod -R ugo+rw /DATA/SHARE

to break it down:

" sudo – this is used to gain admin rights for the command on any system that makes use of sudo (otherwise you'd have to 'su' to root and run the above command without 'sudo')

chmod – the command to modify permissions

-R – this modifies the permission of the parent folder and the child objects within

ugo+rw – this gives User, Group, and Other read and write access."

###   To get a blend of consents you include the numbers together. 

For instance to get read and execute consents the number you require is 5, to get read and compose authorizations the number is 6 and to get compose and execute consents the number is 3

Keep in mind you have to indicate 3 numbers as a major aspect of the chmod command. The first number is for the owner permissions, the second number is for the gathering authorizations and the last number is for every other person.

For instance to get full authorizations on the proprietor, read and execute consents on the gathering and no authorizations for any other individual sort the following= hmod 750 test 

If you wish to change the group name that owns a folder use the chgrp command.

if you don't have the correct permission to create a group you may need to use sudo to gain extra privileges or switch to an account with valid permissions using the su command.

Now you can change the group for a folder by typing= chgrp accounts <foldername>
 

!! ensure you work on doing this on working frameworks like ubuntu to show signs of improvement understanding.


## User Management

To maintain the privacy of users of the Linux system and manage their accounts and supervision, as well as track the problems they will go through, find the solutions to these problems, and inform users the changes, update, and upgrade the system. Even if there is a one user using the system it was always a challenge for the managing users. I will show you some tools and some tasks assigned to add the user to Linux system where you see adding and deleting accounts is an easy process for managing users.

Before we talk about how to creating and deleting a user account I will talk about the most important text file(/etc/passwd) because is processing the all most and necessary details and information about all users in the Linux system.

This file it is open to read only for users, and it is read and writable for root account.

### /etc/passwd file

The /etc/passwd file containe only one separate line, limited by a colon (:) for each user account in the system, also it is stored the information for each user using the Linux system. Every time we add new user to the system all details and information for the new user will stored in the same file. 

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

### Mohamed Mohamed

folder creation and management | Systems Administration

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
