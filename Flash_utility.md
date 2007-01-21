---
title: Flash utility
permalink: Flash_utility
---

About
-----

The flash utility can be used to (re)flash the DTV's FlashROM. This way,
installing new programs as well as replacing emulation ROMs
(BASIC/KERNAL/CHARGEN - also see
[Kernalpatcher](Kernalpatcher "wikilink")) can be done.

The Flash utility has been written by [Daniel
Kahlin/tlr](http://www.kahlin.net/daniel/dtv/).

Features
--------

-   Program (arbitrary) range from buffer
-   Dump range from flash to buffer
-   Verify range against buffer
-   Erase (arbitrary) range
-   Check if range is empty
-   Program sector from buffer
-   Program single byte
-   Dump sector from flash to buffer
-   Verify sector against buffer
-   Erase sector
-   Check if sector is empty
-   Lock down sector (only on Atmel Flashes)
-   Dump complete flash status info including all sector lockdowns
-   Load/save data from/to disk to/from buffer
    -   Loading big files (&gt;202 blocks, &gt;64k) is supported
    -   Data is loaded to an offset relative to buffer start
    -   Raw data as well as PRG format (first two bytes = load address)
        is supported; on PRG load, load adress is ignored
-   Runs with roms disabled
-   Function to display a map of flash usage
-   Supports several flash types, e.g AT47BV161T, AT49BV163A,
    SST39VF1681, etc...
-   1.0pre1 fully supports the SST39VF1681 provided you have this simple
    [hardware fix](http://www.kahlin.net/daniel/dtv/sstfix.php).

Buffer starts at $020000 so the utility can flash at max 1920kByte in
one run.

Known bugs
----------

-   There is only limited sanity checking of the values entered for
    range. If you enter them wrong, it is often best to do
    Esc/Pause-Break, and then SYS 2069 to reenter the program. For
    program range and erase range, simply answering N is sufficient.

Warning
-------

**Note: Erasing/reprogramming sector 00 ($00xxxx/BASIC/KERNAL) is a very
dangerous operation! You may very easily make the DTV unbootable this
way!** Typically only programmers that want to code directly on the DTV
might want to replace the kernal, **everybody else DOES NOT**. There ARE
SEVERAL reports of people having bricked their DTV! Users should use
[DTVTrans](DTVTrans "wikilink") sync instead.

**Note: This utility is fully capable of wiping the entire flash at your
command, making your C64 DTV permanently unbootable!** Make sure you
really know what you are doing! Use at your own risk!

**Test drive the Flash utility in
[VICEplus/x64dtv](http://viceplus.sourceforge.net/) first in any case!**

Download
--------

[flash-1.0.zip](Media:flash-1.0.zip "wikilink") (2008-10-18: fixed
AT49BV163A)

[Browse
repository](http://viceplus.svn.sourceforge.net/viewvc/viceplus/trunk/tools/flash/)

Reporting back
--------------

I'm very interested in reports of success (or failure). Please include
the batch number as printed on the bottom of your unit with reports. If
you have the possibility of looking inside, please send the markings of
the flash chip aswell.
