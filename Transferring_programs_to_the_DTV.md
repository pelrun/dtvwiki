---
title: Transferring programs to the DTV
permalink: Transferring_programs_to_the_DTV
---

Currently there are many ways to transfer programs to your DTV. They
mostly depend on what you want to achieve and the peripherals you have.

Common transfer methods are:

#### Using a DTV Joystick Port (or the User Port)

-   Use [DTVTrans](DTVTrans "wikilink") by Daniel Kahlin (tlr)
    -   Fast (~12kBytes/Sec). PC with parallel port and special cable
        needed. Data can be transferred to the C64DTV's RAM this way.
        Bootstrapping can be done using a small BASIC program (see
        boot.txt included in DTVTrans download).

#### Use the DTV [IEC port](IEC_port "wikilink")

-   With the PC-based parallel [X1541
    cable](http://sta.c64.org/x1541.html) or [XE1541
    cable](http://sta.c64.org/xe1541.html) and some PC-based C64 floppy
    emulation like [64hdd](http://www.64hdd.com/64hdd.html)
    -   Extremely slow, some compatibility issues, DOS/FAT needed
-   With a real C64 floppy drive
    ([1541](http://en.wikipedia.org/wiki/Commodore_1541),
    [1581](http://en.wikipedia.org/wiki/Commodore_1581),...)
    -   Slow but of course highly compatible, using 1541 need a way to
        write data on a floppy (real C64), widespread solution by the
        way.
-   With the [MMC2IEC](http://www.c64-wiki.com/index.php/MMC2IEC) SD &
    MMC based drive
-   With the [1541-III](http://www.c64-wiki.com/index.php/1541-III) SD
    Card based drive

#### DTVLoad

DTVLoad is a transfer program for the Hummer, an addition to the
modified C64 Kernel that is standard in the Hummer ROM.

-   [DTVLoad Details](DTVLoad_Details "wikilink")

Any data loaded into the DTV's RAM can be transferred to its FlashROM
afterwards using the [Flash utility](Flash_utility "wikilink").

Transferring data **without hardware modifications** is not possible
since the DTV does not feature any input connectors.
