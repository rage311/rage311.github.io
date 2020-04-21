---
layout: post
title:  "Getting Started with PC Engines APU2"
date:   2016-05-13 23:52:42 -0600
categories: APU APU2 PC Engines
---
I heard about PC Engines a while back and was intrigued by their (relatively) low-cost, small form factor x86 boards.  All of their boards are essentially purpose-built to be routers/firewalls, due to the form factor, multiple NIC setup, lack of display outputs, and low power usage.  They got fairly popular with the pfSense crowd with their [ALIX](http://pcengines.ch/alix.htm) boards a few years back, but those are seemingly underpowered if you want to use it for much more than basic firewall/router capabilities.

I looked at PC Engines a couple years back when the APU1 was their top of the line board.  While it intrigued me, I still had a Pentium 4 based pfSense box that was running just fine (albeit with a much higher power consumption than I'd like).  When the APU2B was released at the end of 2015 however, I really started thinking about replacing my aging, power-hungry box.  And when the APU2C series was released early this year with a quad core 1 GHz CPU, 3 **Intel** NICS (instead of the previously-used Realteks), and the option for 4GB of ECC RAM, I decided I had to have one.  I went with the APU2C4 for the 4GB of ECC RAM and the more capable Intel i210AT NICS.

Here's a comparison between APU2C2 and APU2C4 from [PC Engines site](http://pcengines.ch/apu2c4.htm):
>apu2c2 = 3 i211AT LAN / AMD GX-412TC CPU / 2 GB DRAM

> apu2c4 = 3 i210AT LAN / AMD GX-412TC CPU / 4 GB DRAM with ECC
USB boot drive (1GB+) for BIOS updates/installation media

The PC Engines boards are DIY-style boards.  You have to assemble the parts yourself -- the board with embedded CPU, the SSD, the heatsink, PCIe wifi card and antennas, and the case.  Fortunately they've got decent-enough assembly instructions on their website (mainly for the heatsink portion).

The rest of the documentation on how to get started with using the board seemed a little lacking and scattered.  So I thought I should put together a small, fairly straightforward guide to help string some of the pieces together.

### Getting Started

Assemble the board, heatsink, and case according to the [official instructions](http://pcengines.ch/apucool.htm).

Plug a **null modem** cable between your machine and the APU.  I don't have an RS-232 port, so I'm using a USB to RS-232 converter.  Mine has the Prolific PL2303HX chip.  [Here's my exact converter on Amazon](http://www.amazon.com/Plugable-Adapter-Prolific-PL2303HX-Chipset/dp/B00425S1H8).

Our first goal is to update the BIOS to the latest version.  You might as well do this before you have too much to lose.

* [Download PC Engines' Tinycore Linux](http://pcengines.ch/howto.htm#TinyCoreLinux) (apu2-tinycoreX.X.img.gz), which contains `flashrom`
* `dd` the .img file to a USB flash drive:
  * `# gunzip -c tinycoreX.X.img.gz | dd of=/dev/sdX bs=1M; sync`
(add `status=progress` as an option to `dd` if using GNU `dd` for a progress readout)
* Download the [latest BIOS version](http://pcengines.ch/howto.htm#bios), extract the .rom file, and copy it to the Tinycore flash drive
* Plug the flash drive into the APU and connect the power to turn it on and start the boot process.
* Open a connection to the APU's serial console.  You may have to push return a couple times to get any response.  I had to power cycle my APU again to see anything from it.
  * `$ screen /dev/ttyUSB0 115200`
  * Your user must be member of the `uucp` group in Arch Linux -- `usermod -aG uucp myusername`
  * Check here to verify the necessary group:
    * `ls -l /dev/ttyUSB0` outputs `crw-rw---- 1 root uucp 188, 0 Apr 20 21:26 /dev/ttyUSB0`
* When it boots, you'll see a prompt to hit F10 for options.  This will bring up a menu of boot options, with one of them being memtest.  I've had enough RAM issues that I run a memtest on all new RAM... but this step is optional:
  * F10 > memtest
* After completion, reboot, hit F10 again, and select the USB flash drive.  At this point, it should boot into Tinycore Linux.
*  I'm not sure why, but I got an error when Tinycore booted
`Error: \"unable to mount device\", \"unable to find autoboot.sh\"`.  The file is located in `/media/SYSLINUX` (my version is `apu2-tinycore6.4.img` by the way).  To proceed:
  * `# cd /media/SYSLINUX`
  * `# ./autoboot.sh`
  * `# flashrom -w apu2_160311.rom -p internal`
  * `# reboot`

Hopefully, you now have an updated, functional board with known good RAM, ready for an operating system like: pfSense, FreeBSD, OpenBSD, Linux, and several Linux-based router distributions.  I chose OpenBSD for this duty.  I'll cover that installation and configuration process in the next few posts... which might get lengthy.  The topics will include: pf as the firewall, dnsmasq for a DHCP server and local hostname registration and resolution via DNS, unbound for the DNS resolver, dnscrypt proxy for encrypted DNS lookups, and more.
