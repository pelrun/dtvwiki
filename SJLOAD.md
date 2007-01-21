---
title: SJLOAD
permalink: SJLOAD
---

About
-----

SJLOAD is a C64/C64DTV software
[fastloader](http://www.c64-wiki.com/index.php/Fast_loader). Its main
difference from normal fastloaders is that it only works with
Jiffy-enabled drives and uses the Jiffy protocol. This makes it handy
for people who have a Jiffy-enabled drive (also new hardware such as
[SD2IEC](http://www.c64-wiki.com/index.php/SD2IEC)) but do not want to
make the hardware changes necessary for exchanging the C64 kernal.

SJLOAD speed is a bit higher than normal Jiffy since SJLOAD uses the
same protocol but a different implementation (it disables the VICII
during load etc.). With an SD2IEC, SJLOAD is about 15% faster than a
normal Jiffy kernal. Note that SJLOAD **does not** feature the command
wedge and function key shortcuts known from a Jiffy kernal.

SJLOAD is loosely based on VDOS (1986) by Edward Carroll. However, the
fast loading routines have been replaced completely by
[1570](User%3A1570 "wikilink")
([contact](Special:Emailuser/1570 "wikilink")).

Usage
-----

-   LOAD“!”,8,1 - autostart SJLOAD
-   LOAD“!\*PROGRAM”,8,1 - autostart SJLOAD, fastload PROGRAM
    -   Since 1581 (compatible) drives do not stop filename matching at
        “\*”, use LOAD“!=PROGRAM”,8,1 on these drives.
-   LOAD“!”,8:REM CHANGE DISK:RUN - save autostarting SJLOAD to (new)
    disk
-   VERIFY - read floppy status - VERIFY"",9 reads status from drive 9
-   VERIFY“*command*” - send floppy command
-   VERIFY“$” - display directory - scroll to entry and press
    SHIFT+RUN/STOP to load and run program

If a program crashes on RUN after loading it using SJLOAD, try
RUN/STOP+RESTORE (this disables SJLOAD) before RUNning it.

Status
------

**[ChangeLog](http://picobay.com/dtv_wiki/index.php?title=SJLOAD/SJLOAD.ASM&action=history)**

SJLOAD is far from completed:

-   Loading files bigger than 195 blocks makes the C64 crash
-   Loading files below $0801 is not supported
-   Only few tests have been made (with a C64DTV and an SD2IEC and in
    VICE)
-   No IEC timing fixes for the C64DTV are done so the
    J1541/J1571+C64DTV combo does not work with SJLOAD (J1581 and SD2IEC
    *do* work). See [JiffyDOS](JiffyDOS "wikilink") for a DTV solution.

I (1570) probably will not work on SJLOAD any more. The source is there,
fix it! :-)

Download
--------

-   [SJLOAD.D64](:Media:SJLOAD.D64 "wikilink") including source for
    SJLOAD and VDOS. Also includes CRCCHECK, a testing program that
    repeatedly loads itself and checks its integrity.
-   [SJLOAD/SJLOAD.ASM](SJLOAD/SJLOAD.ASM "wikilink") - SJLOAD source in
    the wiki (just edit it if you have fixes at hand).

Links
-----

-   [Petscii Forums: Thread “SJLOAD: Software fastloader for Jiffy
    drives”](http://jledger.proboards.com/index.cgi?board=dtvhacking&action=display&thread=2830)
-   [Retrohackers.org: Thread “SJLOAD: Software fastloader for Jiffy
    drives”](http://retrohackers.org/viewtopic.php?f=6&t=427)
-   [Forum64.de: Thread “SJLOAD: Software-Schnelllader für
    Jiffy-Laufwerke”](http://www.forum64.de/wbb3/index.php?page=Thread&threadID=27231)

<!-- -->

-   [JiffySoft128](http://sites.google.com/site/h2obsession/CBM/C128/JiffySoft128) -
    a similar C-128 implementation
-   [SJLOAD-20](http://retrohackers.org/viewtopic.php?p=4124#p4124) - a
    similar VC-20 implementation

