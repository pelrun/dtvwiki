---
title: DTVSlimIntro
permalink: DTVSlimIntro
---

About
-----

DTVSlimIntro is a replacement for the original INTRO file in the DTV's
flash. It is based on Roland's INTRO ([Forum64.de
thread](http://www.forum64.de/wbb2/thread.php?threadid=9737)).
Additionally to listing and starting programs, DTV palette can be chosen
out of four presets as well as completely customized.

-   Palette A:
    [Luminance-fixed](http://jledger.proboards19.com/index.cgi?board=dtvhacking&action=display&thread=1896&page=1#19314)
    [Spiff
    palette](http://jledger.proboards19.com/index.cgi?board=dtvhacking&action=display&thread=1896)
-   Palette B: [Zee
    palette](http://jledger.proboards19.com/index.cgi?board=dtvhacking&action=display&thread=1471&page=1#9645)
    (default)
-   Palette C: 1570 palette
-   Palette D: [C64DTV default
    palette](DTV2_Kernal_disassembly "wikilink")

Usage
-----

Put the file into the DTV's flash (for example using “Import PRG” in
[DTVFSEdit](DTVFSEdit "wikilink")). It will display any files in Flash
that use a start address different from 0. If start address is
$0100/256, it will be started using BASIC RUN.

### Go to BASIC

There's no “go to BASIC” option in DTVSlimIntro. However, you can simply
put a dummy entry into the flash with start address 58260 ($E394: BASIC
Cold Start). [Spiff ZIP](Media:basic_dtv.zip "wikilink") containing this
trick.

### PAL output settings ($D04E)

In the file listing, pressing the DTV buttons changes the DTV's D04E
value (A=0,...,D=3). Some FBAS=&gt;VGA converters are known not to work
properly with the DTV's default setting (D04E=0) but with D04E=2 or 3.
However, other devices have issues with these settings again.

If you want to hardcode a default value in DTVSlimIntro, use a hex
editor to replace $2c at offset $7cf in the file by $8d, and put the
D04E value of choice to $7ce. In the source, see the code after label
*mstart*.

### Autostart

DTVSlimIntro will autostart any file named AUTOSTART unless a key is
pressed or joy2 is moved/a button pressed while DTVSlimIntro starts.
This makes defaulting to BASIC, a modified softkernal, or the original
DTVMENU quite easy.

Changes
-------

-   2009-09-19: added AUTOSTART feature, support movement using cursor
    keys (file list only)
-   2008-10-11: added PAL D04E fix needed for some FBAS=&gt;VGA
    converters, fixed palettes, faster start
-   2007-11-03: Enable skip+burst for LOAD - big speedup for files that
    have not been compressed apart from the DTV Flash compression
-   2007-09-20: Can load large files (extending above $0110f7), minor
    palette change

### Todo

Not implemented yet (any takers?).

-   Parse “$” instead of accessing FlashROM
    -   Will make the program independent from individual FlashFS
        implementation (see [DTV Flash File
        System](DTV_Flash_File_System "wikilink")).

Download
--------

-   [dtvslimintro.prg](https://viceplus.svn.sourceforge.net/svnroot/viceplus/trunk/tools/dtvslimintro/dtvslimintro.prg) -
    Put as INTRO into Flash. Load to $0801.
-   [SVN
    repository](http://viceplus.svn.sourceforge.net/viewvc/viceplus/trunk/tools/dtvslimintro/)

