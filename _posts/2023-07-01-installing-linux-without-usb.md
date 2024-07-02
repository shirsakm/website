---
layout: post
author: Shirsak Majumder
---

In this guide, I will be guiding you through the process of installing Linux distributions, without using a USB stick, CD or DVD. We will be booting into a live Linux distribution, directly from the hard drive using an _EFI file_. This guide will work for all _UEFI_ systems, and all Linux distributions that include an _EFI file_ in their `.iso` image. My poison of choice for this article will be EndeavourOS.

Next, I will be discussing the pros, as well as the cons, of booting via this method, and give some background information on the technologies used here, following it up with my personal experience. If you have read this before, are familiar with this method, or have all prerequisite background infrormation, you can skip directly to [the installation steps](#installation).

## Table of Contents
- [Table of Contents](#table-of-contents)
- [Is this method right for you?](#is-this-method-right-for-you)
  - [Advantages](#advantages)
  - [Disadvantages](#disadvantages)
- [Background](#background)
  - [Linux](#linux)
  - [EFI file](#efi-file)
  - [EndeavourOS](#endeavouros)
- [My Experience](#my-experience)
- [Prerequisites](#prerequisites)
- [Installation](#installation)
  - [Making the bootable EFI partition](#making-the-bootable-efi-partition)
  - [Booting into the live Linux distribution](#booting-into-the-live-linux-distribution)
  - [Installing EndeavourOS](#installing-endeavouros)
- [Conclusion](#conclusion)

## Is this method right for you?

### Advantages

While installing via a USB stick is recommended, there are many reasons to want to install without one, as mentioned below,

1. Exorbitant prices of USB sticks in developing countries.
2. Dysfunctional CD/DVD reader.
3. Rewritable and reusable unlike CD-R media.

### Disadvantages

This method also comes with some serious disadvantages,

1. Only works on _UEFI_ systems.
2. Unable to install distributions without a bootable _EFI file_ in their `.iso` image.
3. Unable to install distributions that prevent installing to the same hard drive.

## Background

### Linux

Linux is a family of _open-source_ Unix-like operating systems based on the Linux kernel, known for it's versatility and customisability. From heavily customised, and Windows-like experiences, such as Zorin OS, Deepin OS and Linux Mint, to the most bare-bones builds, such as Arch Linux, Gentoo Linux, and Void Linux. No matter what you need, Linux has it all, and I recommend you pick your poison wisely!

### EFI file

An EFI file is a _bootloader executable_, compatible only with UEFI based systems, and contain data on how the boot process should proceed. They have a `.efi` extension, and will likely be in the `EFI` folder of your `.iso` image.

### EndeavourOS

Based on Arch Linux, EndeavourOS has a similar, minimalist, terminal-centric approach. However, it makes the wonderful world of Arch Linux available with a very user-friendly, as well as beginner-friendly user experience. Learn more on [the official website](https://endeavouros.com/).

## My Experience

Here, I will be discussing my personal experience with this method, installing various Linux distributions, on my [HP Notebook 15-ay453TU](https://support.hp.com/in-en/document/c05366603). I find this method extremely simple, successfully installing Linux distributions from Ubuntu to Arch Linux. However, the method certainly has prominent disadvantages, primarily limiting the options of Linux distributions you can install to your machine. [Fedora Linux](https://fedoraproject.org/), a popular Linux distribution known for it's stability, refuses to install to the same media it was installed on, hence, I was unable to install it, via this method. In the same vein, [Manjaro](https://manjaro.org), another Arch Linux based distribution, does not contain a bootable EFI file, rendering this method useless. However, if this limiting of choice is not a problem, and the system is UEFI based, this method works flawlessly.

## Prerequisites

There is only one hardware prerequisite for this method,

- UEFI based system

The rest of the prerequisites will largely depend on the Linux distrubtion you choose,

- technical knowledge
: Your choice of distribution, entirely dictates how much technical prowess you require. While user-friendly distributions, like Ubuntu and EndeavourOS, may require hardly any, highly technical distributions, like Arch Linux and Gentoo Linux, are likely to require a lot. While following steps on a Wiki tutorial hardly requires any knowledge, solving any problems you run into, will certainly require it.

- hard drive space
: This will include the size of the `.iso` image after extraction, 1GB of space for the EFI partition, as well as the minimum hard drive space required by the distribution. 

All other minimum system requirements are trivial to this method, and should be confirmed from the source of the Linux distribution.

## Installation

### Making the bootable EFI partition

1. Install the `.iso` image for the Linux distribution of your choice. If you would like to follow along faithfully, please install the [EndeavourOS `.iso` image](https://endeavouros.com/#Download). 

2. Next, we will be creating a partition for the bootable EFI file.
: For Windows press `Win` + `R` and run `diskmgmt.msc`. From here, create a new `FAT32` volume of the required size, ideally 8GB. **Remember the partition name, this will be required later.** I name it `IMG` for convenience sake.
: &#10240;
: For Linux, I recommend installing GParted, and creating a `FAT32` partition of the required size, ideally 8GB. **Remember the partition name, this will be required later.** I name it `IMG` for convenience sake.
: &#10240;

3. Now, we will be mounting the `.iso` image and copying them over to the partition.
: For Windows, simply double click the `.iso` file to mount it. Press `Ctrl` + `A` to select all files, and copy them to the new volume.
: &#10240;
: For Linux, you have a variety of options, but I recommend using the `mount` command, and mounting the `.iso` image to the required partition.
```sh
$ sudo mount /dev/sdX /mnt
$ sudo mount -o loop image.iso /mnt
$ sudo umount /mnt
```

### Booting into the live Linux distribution

1. The first step here, is to access the _Change Boot Order_ menu. When your computer is booting, a key on your keyboard needs to be pressed. The particular key entirely depends on the manufactuer of your system, and will require research on your part to find, but you can find some common keys in [this article by HP](https://www.hp.com/us-en/shop/tech-takes/how-to-enter-bios-setup-windows-pcs). On HP Notebooks, press `Esc` followed by `F9`, this will bring you to the menu. Here select the `Boot from EFI partition` option.

2. Your system should now boot into a boot manager, which will be `grub` for most distributions, but is `systemd-boot` for EndeavourOS. Here, press `e` and you will have the option to edit the command. **Replace the name of the Linux distribution with the name of the partition you created.** For EndeavourOS,we are looking to edit the `archisolabel` variable.

### Installing EndeavourOS

You should have booted into a live session of the Linux distribution of your choice. From tis point forth, please follow the installation instructions provided by your Linux distribution Wiki. If no instructions are provided, you can still try to follow along, as the main issue is likely to be in making partitions, but I will be continuing with EndeavourOS, which already has a very intuitive installer.

1. Run _Update Mirrors (Arch, reflector-simple)_ and _Update Mirrors (EndeavourOS)_, to update the mirrorlist for Arch Linux, as well EndeavourOS packages. While this step is optional, it is highly recommended if you want an online install.

2. Run _Partition Manager (gparted)_, and create a new `ext4` partition of the required size, at least 15GB. This will be the partition the file system will be using, so, be generous with the size. Alongside, create a 1GB `FAT32` partition for the EFI mount point.

3. Click on _Start the installer_ and _Online_. You can also have an _Offline_ install, where you will have the _Plasma KDE_ desktop environment, and will have to update all packages after installing. If you go that route, most of the steps will still apply, especially the _Partitions_ step.

4. Once _Calameres_ installer opens, set your location and keyboard, as required.

5. When it comes to desktop, if you are confused, pick _Plasma KDE_, if you are not, pick your poison. My poison is `i3-wm`.

6. If you are not sure, simply install the default packages, however, I do install the _LTS kernel in addition_, but that is only a personal preference.

7. In the _Bootloader_ section, I recommend going with the default `systemd-boot`, which will take 1GB of space. If you are crammed for space, you should select `grub` instead, as it requires 300MB of space instead.

8. In the _Partitions_ section, select _Replace a partition_ and select the partition you just created in GParted. You **cannot erase disk in this method**, as you are booting from the disk.

9. In the _Users_ section, simply fill in the form, and set a password.

10. _Calameres_ installer will now show you a summary of the install, and the changes to be made. Confirm the install, and wait for the installer to complete.

## Conclusion

If you dual-booted with Windows, your system might be booting into Windows Boot Manager automatically. To prevent this, once again, you will have to enter _UEFI firmware settings_ using either, the respective key for your manufacturer, or using Windows, and change the boot order, placing your install's bootloader over Windows Boot Manager.

Voilà! You now have Linux installed and if you followed this guide to a T, you should now have EndeavourOS installed on your system, without a USB or CD/DVD.
