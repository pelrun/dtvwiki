---
title: Hummer
permalink: Hummer
---

Brief Summary
-------------

The Hummer Off-Road Racing Challenge game was sold by RadioShack for the
2005 Christmas season, it is no longer available through retail
channels. While it was not sold as a Commodore device at all, it
contains a version 3 DTV ASIC, 2MB RAM and 2MB FLASH ROM. It comes with
the Hummer Off-Road Racing Challenge game, which was written
specifically for the Hummer hardware. The Hummer off-road racing
hardware is functionally equivalent to the DTV3 with the following
notable exceptions, different timing Crystal, only 3 of 10 joystick I/O
lines and an ADC included on-board. There are additional minimal
differences in software related to PAL & NTSC issues. Since the Hummer
lacks complete joystick ports a gaming solution has been developed which
allows specially [ patched games](Userport_Patched_Games "wikilink")
that receive control from the userport to be played. The Hummer hardware
also has the same video flaw (and same fix available) as the version 2/3
joystick sold in Europe.

Specifications
--------------

Image:PRS1C-2356119\_rshalt2\_dt.jpg|Hummer
Image:PRS1C-2356119\_rshalt1\_dt.jpg|Hummer, boxed

-   DTV3 core.
-   2 MiB RAM
-   2 MiB FlashROM ([Atmel](http://www.atmel.com/)
    [AT49BV163A](http://www.atmel.com/dyn/resources/prod_documents/doc3349.pdf))
-   Video output with programmabe color carrier and PAL/NTSC timing.
-   6502 emulation supporting many undocumented opcodes.
-   6502 skip internal cycle and burst mode selectable.
-   Steering wheel connected to an ADC (equivalent to a
    [Sonix](http://www.sonix.com.tw/)
    [SNAD01C](http://www.datasheetsite.com/datasheet/SNAD01C)) START,
    CLK, DIO are hooked up to USR2-0.
-   Two steering wheel buttons (mapped as Delete and Return on the
    keyboard)

External Ports
--------------

-   Composite video
-   Audio

Internal Ports
--------------

These are only available by soldering.

-   PS/2 port (emulates the C64 keyboard matrix)
-   IEC port for connection of peripherals.
-   User port (8 bits)
-   Control port 1 ( 2 bits Up & Down only, JoyB0 and JoyB1 )
-   Control port 2 ( 1 bit Up only, JoyA0)

Howto
-----

**[Hummer
FAQ](http://home.earthlink.net/~dgdtv/dtv/data/hummer_faq.html)**

[Connect a Keyboard](Connect_a_Keyboard "wikilink")

[Connect a Disk Drive](Connect_a_Disk_Drive "wikilink")

[Fix the Audio Circuit](Fix_the_Audio_Circuit "wikilink")

[Fix the Video Circuit](Fix_the_Video_Circuit "wikilink")

[Convert the Userport to a Joystick
Port](Convert_the_Userport_to_a_Joystick_Port "wikilink")

[Flash the Kernal](Flash_the_Kernal "wikilink")

[Add Software to the Flash](Add_Software_to_the_Flash "wikilink") - for
the DTV, this is covered on [Flash the DTV Rom\#Flash new
programs](Flash_the_DTV_Rom#Flash_new_programs "wikilink"). However, the
Hummer uses a slightly different [flash file
format](Hummer_Kernal_disassembly#File_Format "wikilink") for which no
tools (dtvpack/[DTVFSEdit](DTVFSEdit "wikilink")) exist. Just flash a
DTV2 kernal modified by [Kernalpatcher](Kernalpatcher "wikilink") - then
your Hummer will use the DTV2/3 file system.
[DTVSlimIntro](DTVSlimIntro "wikilink") can be used for starting
programs then. Navigation in the intro is possible using cursor keys -
userport joystick is not supported.

[Power Supply Options](Power_Supply_Options "wikilink")

[Hummer Userport Patching](Hummer_Userport_Patching "wikilink"): for
those who want to patch their own games.

[Pinouts and PCB Connects](:Image:Connectors.png "wikilink")

[Userport Patched Games](Userport_Patched_Games "wikilink")

[Hummer Kernal disassembly](Hummer_Kernal_disassembly "wikilink")
