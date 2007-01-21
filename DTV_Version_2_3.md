---
title: DTV Version 2/3
permalink: DTV_Version_2/3
---

![](Pal-dtv-v3.jpg "Pal-dtv-v3.jpg")

Also see [Wikipedia article](http://en.wikipedia.org/wiki/C64DTV).

Specifications
==============

-   Housed in a joystick.
-   Two fire buttons.
-   4 general buttons (A/B/C/D)
-   Reset button.
-   DTV2 or DTV3 core
    -   the DTV2's Blitter has problem with transparent blits (writes
        occur when srcA is fetched regardless of transparency setting)
        which is fixed in DTV3
-   2 MiB RAM (ISSI
    [IS42S16100C1-7T](http://www.issi.com/pdf/42S16100C1.pdf))
-   2 MiB FlashROM (Atmel
    [AT47BV161T](:media:AT47BV161T.pdf "wikilink"),
    [SST39VF1681](http://www.sst.com/dotAsset/40770.pdf), or very rarely
    Atmel
    [AT49BV163A](http://www.atmel.com/dyn/resources/prod_documents/doc1427.pdf))
-   Video output with programmable color carrier and PAL/NTSC timing.
    (Only retailed as PAL)
    -   All DTV2/3s are shipped with a minor fault in the video circuit
        leading to relatively bad colors. This can be fixed. See [DTV
        Mods](DTV_Mods "wikilink").
-   6502 emulation supporting many undocumented opcodes.
-   6502 skip internal cycle and burst mode selectable.

External Ports
--------------

-   Composite video
-   Audio

Internal Ports
--------------

These are only available by soldering - see [DTV
Mods](DTV_Mods "wikilink").

-   [Keyboard port](Keyboard_port "wikilink") (emulates the C64 keyboard
    matrix)
-   [IEC port](IEC_port "wikilink") for connection of peripherals.
-   User port
-   S-Video
-   Cassette port sense line
-   [Joystick port](Joystick_port "wikilink") 1 & 2 (all 5 connections)

Games
-----

AlleyKat, California Games, Championship Wrestling, Cybernoid, Cybernoid
II, Cyberdyne Warrior, Eliminator, Exolon, Firelord, Gateway to Apshai,
Head The Ball, Impossible Mission, Impossible Mission 2, Jumpman Jr.,
Marauder, Maze Mania, Mission Impossibubble, Nebulus, Netherworld,
Paradroid, Pitstop, Pitstop II, Ranarama, Speedball, Summer Games, Super
Cycle, Sword of Fargoal, Uridium, Winter Games, Zynaps

Revisions
=========

The batch number is “hotstamped” into the plastic on the bottom of the
joystick just beside the battery compartment, as shown below. It is not
identical to the batch number stamped onto the retail packaging. The
assumed interpretation is YYMMDD.

![DTV batch number](DTV_v3_batchnum.jpg "DTV batch number")

| Batch  | Core       | Flash                                            | Reporter                                    |
|--------|------------|--------------------------------------------------|---------------------------------------------|
| 050711 | not tested | not tested                                       | (David Murray)                              |
| 050728 | DTV2       | AT47BV161T                                       | (huckle)                                    |
| 050910 | not tested | not tested                                       | (zitev)                                     |
| 050917 | not tested | AT47BV161T                                       | (zitev)                                     |
| 050919 | DTV2       | AT47BV161T                                       | (Roland, x1541, zee, expertsetup)           |
| 050921 | DTV2       | AT47BV161T                                       | (Roland, Fröhn)                             |
| 050927 | DTV2       | AT47BV161T                                       | (Madmodder, nojoopa)                        |
| | DTV3 | AT47BV161T | (jsaily, grokk)                                  |
| 051005 | DTV3       | AT47BV161T                                       | (tlr, SubZero)                              |
| | DTV2 | not tested | (bordeaux)                                       |
| 051008 | DTV2       | AT47BV161T                                       | (1570, GerdC128)                            |
| | DTV3 | AT47BV161T | (MasterHit, spiff, peiselulli, twoflower, larsp) |
| | DTV2 | AT49BV163A | (1570)                                           |
| 051103 | DTV3       | AT47BV161T                                       | (spiff, abraxxl)                            |
| 060118 | DTV3       | SST39VF1681                                      | (LogicDeLuxe, Strider, Tobe, 1570, abraxxl) |

Note that there seem to be both DTV2s and DTV3s in batch 050927, 051005
and 051008 at least so batch number is not absolutely reliable for
determining the DTV version. You have to run DTVDetect or Blitterscroll
by tlr to determine DTV version reliably. Bordeaux reports that his
051005 DTVs came in a white box, so buying according to box color alone
does not guarantee a V3 DTV.

The **AT47BV161T** ([data sheet](:media:AT47BV161T.pdf "wikilink")) was
never released to the public. It is pin compatible to the AT49BV161T and
AT49BV163AT ([data
sheet](http://www.atmel.com/dyn/resources/prod_documents/doc1427.pdf))
but lacks some of its features and is guaranteed to last 100 flash erase
cycles only. See [this
thread](http://jledger.proboards19.com/index.cgi?board=dtvhacking&action=display&thread=1135201638&page=2)
for details. [This
thread](http://jledger.proboards19.com/index.cgi?board=dtvhacking&action=display&thread=1133731077)
describes programming details from the software perspective.

For DTVs using the **SST39VF1681** chip a [hardware
modification](http://www.kahlin.net/daniel/dtv/sstfix.php) is needed for
reflashing the device (add a 47pF capacitor between CS and GND or Vcc).
Also, recent flash software (June 2007 or newer) is needed.

See [Flash the DTV Rom](Flash_the_DTV_Rom "wikilink") for details on the
flashing procedure.

FAQ
===

How to attach a keyboard/joysticks/floppy/...?
----------------------------------------------

See [DTV Mods](DTV_Mods "wikilink").

How can I put new games on the DTV?
-----------------------------------

See [Flash the DTV Rom\#Flash new
programs](Flash_the_DTV_Rom#Flash_new_programs "wikilink").

Can I use JiffyDOS with the DTV?
--------------------------------

Yes. Use **[this version](http://dtvforge.ath.cx/jiffydtv/)** that is
patched specifically for the DTV and fixes some IEC timing bugs. Just
run the program you get from that page on the C64DTV. It will put the
Jiffy kernal to RAM $1EE000 and use the [DTV memory
mapper](DTV_Programming#MEMORY_MAPPER "wikilink") to 'install' the new
kernal (temporarily).

If you want to use the Jiffy kernal by default, use
[DTVSlimIntro](DTVSlimIntro "wikilink") and put the Jiffy kernal as
AUTOSTART into flash.

How can I reflash the DTV to boot to BASIC/Jiffy/... instead of showing the menu?
---------------------------------------------------------------------------------

Simply use the AUTOSTART feature of
[DTVSlimIntro](DTVSlimIntro "wikilink").

Quality of video output is bad. What can I do?
----------------------------------------------

The DTV's video timing is not completely PAL compliant. In general, CRTs
work much better with the DTV's signal than flat screens. If the picture
does not synchronize properly, try using
[DTVSlimIntro](DTVSlimIntro "wikilink")'s D04E changing feature (or POKE
manually). The hardware colorfix and S-Video output (see [DTV
Mods](DTV_Mods "wikilink")) might also help a bit. However, picture
quality is mostly determined by the screen's input circuit.

Some people have modded the DTV to NTSC output. This requires replacing
the timing crystal with a hard-to-get NTSC variant and reflashing the
kernal. The NTSC output has far fewer issues. However, this is not
recommended as PAL programs might not run properly on an NTSC C64(DTV).

How compatible is the DTV with the C64?
---------------------------------------

The DTV has a few but crucial [emulation
issues](DTV_Programming#C64_emulation_issues_(for_the_DTV2/3) "wikilink").
Many games (say, 10 to 30%) do not work properly. Old games are just as
susceptible not to work as are newer ones: For example, Bruce Lee does
not work without patches at all (but was easy to fix) while Mayhem in
Monsterland 'just' has graphical glitches (that were very hard to fix).
See also [DTV Patched Games](DTV_Patched_Games "wikilink").

Is a DTV3 much better than a DTV2?
----------------------------------

No. The only difference is the fixed blitter bug and a better flash
chip. The blitter bug only affects a few DTV specific demos. So far, the
inferior flash chip of the DTV2 has not failed anywhere.

Some games from Spiff's Repository fail to run properly. Why is that?
---------------------------------------------------------------------

1. There are several fixed games in [Spiff's
repository](http://symlink.dk/nostalgia/dtv/fixed/) that are large files
(i.e., files that are longer than 202 blocks/51k/extend above $CFFF).
These files are intended to be flashed and then loaded from flash.
LOAD“...”,1 always loads file data to RAM whereas LOAD“...”,8 writes to
the memory visible to the CPU. For the CPU, at $D000 starts the I/O area
- writing random values there makes the C64(DTV) crash.

Solution:

-   Flash the game
-   OR use a big file compliant LOAD
    -   [DTV Speed Load](http://noname.c64.org/csdb/release/?id=40464) -
        needs a normal floppy drive, you may run into problems with the
        160k disk limit here
    -   The [TRSI kernal](http://noname.c64.org/csdb/release/?id=71856)
        speed load is also supposed to load big files properly
    -   Advanced: A [patched kernal](Kernalpatcher "wikilink"), possibly
        softkernal, including the big file patch from SVN
-   OR use [DTVTrans](DTVTrans "wikilink") to transfer the file directly
    from a PC to the DTV's RAM
-   In VICE/x64dtv, you can load big files from the monitor: *bank ram,
    l “file” 0, x*

2. There also some fixed games that come in multiple files and load game
data from flash. You have to put these in flash.

External Links
==============

Note: these two links should be moved--each refers to Hummer as well as
C64DTV, and the programming document (which covers all DTV versions) was
already posted at [DTV Specific
Programming](DTV_Specific_Programming "wikilink"), under DTV
Programming.

-   DTV2 Schematic: <Media:DTV-Ver2-Schematic.pdf>
-   DTV2 Programming: <Media:DTV-Version2-Programming.pdf>

