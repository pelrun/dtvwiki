---
title: DTV Mods
permalink: DTV_Mods
---

Getting started
---------------

#### Quick test

If you just want to have a quick look without any hardware
modifications, you can go to a BASIC prompt using the built-in [easter
egg](DTV_Easter_Eggs "wikilink"):

-   In the blue “READY.” screen wiggle the joystick left-right or
    up-down very quickly.
-   Then, *LOAD“$”,8* instead of *LOAD“\*”,8,1* will be run, giving a
    list of programs that can be run using the joystick, most notably
    the BASIC Prompt which features a keyboard emulation that can be
    accessed with the joystick.

#### Connecting the DTV to the outside world

To get data on the DTV, you need to add at least :

-   a keyboard port (see [keyboard mod](Keyboard_port "wikilink"))
-   a joystick connector OR a floppy/IEC connector (see [joystick
    mod](Joystick_port "wikilink")/[floppy mod](IEC_port "wikilink"))

#### Power supply

Make sure the keyboard gets 4.5 to 5 Volts DC. 6 Volts (4x1.5 Volts
normal batteries) will probably fry it!

-   Use a regulated/stabilized 5V DC power supply, 500mA or more
    -   USB chargers and some mobile phone chargers should be okay.
-   Or rechargeable batteries (4x1.2V)
-   Or a standard/non-regulated power supply (12V,...) with a [extra
    regulator](http://www.tkk.fi/Misc/Electronics/circuits/psu_5v.html)
    to 5V. Add a 1N4148 protection diode pointing from pin 3 to pin 1
    (see [here
    (German)](http://www.forum64.de/wbb3/index.php?page=Thread&postID=152909#post152909)
    why )

The DTV board is designed to work with a 3.3V voltage. The built-in
**red power LED** works as voltage reference in the DTV's [3.3V
regulator circuit](:Image:dtv2_power_circuit.png "wikilink") - so don't
remove or replace it with a different LED type (1.8V forward voltage
needed - especially blue LEDs are not suitable). Replacing this
regulation circuit with a linear regulator is possible but seems to
reduce power consumption only by about 15% (from ~166mA to ~140mA -
[PETSCII forum
thread](http://jledger.proboards.com/index.cgi?board=dtvhacking&action=display&thread=1555&page=1#10656)
).

The DTV **power switch** wiring is a bit unusual. It connects DTV
*ground* (“E-”) to battery ground in “On” position. In “Off” position,
it connects the 3.3V DTV line to DTV ground via a 100 Ohms resistor
(“Dis”), probably to discharge capacitors. Keep this in mind when
attaching peripherals whose ground connection might bypass the power
switch.

#### Transferring data to the DTV

See [Transferring programs to the
DTV](Transferring_programs_to_the_DTV "wikilink").

DTV Hardware Mods
-----------------

*please add only 'best' links here - this is not intended as a
'complete' link collection*
<img src="SpiffColorFix.png" title="fig:Simple Spiff Color Fix" alt="Simple Spiff Color Fix" width="200" />
<img src="C64DTV_solderpoints.jpg" title="fig:Solder points" alt="Solder points" width="200" />

-   [Simple colorfix](http://symlink.dk/nostalgia/dtv/colorfix/) for
    DTV2/PAL. In a nutshell: Add three 220Ω SMD resistors (0603
    packaging; 0805 also fits) *in parallel* to the existing R16, R20,
    and R24. Also, add one 330Ω resistor (a standard wired one works)
    from the point between R14 and R16 (the side of R16 pointing away
    from R20 is okay) to ground. Use a grounding point a bit away from
    the audio line to prevent noise. 166Ω can be measured on each of the
    additional resistors if soldering is okay. Reported to work even
    better than the ['true'
    colorfix](http://www.kahlin.net/daniel/dtv/colorfix.php) as
    suggested by Mr. Latchup which requires replacement of 17 (!) SMD
    resistors. Good SMD soldering skills and equipment needed!
    [Comparison](http://jledger.proboards19.com/index.cgi?action=display&board=dtvhacking&thread=1157288083)
    of palettes. Note that for perfect colors the software palette needs
    to be changed, too ([DTVSlimIntro](DTVSlimIntro "wikilink") can do
    this).
-   [S-Video](http://galaxy22.dyndns.org/dtv/common/svideo/index.html)
    output for clearer video (for Hummer. Remove
    [C10](http://www.kahlin.net/daniel/dtv/misc/dtv_v2_video-fixed-sch.png)
    for DTV2)
-   [DTV2 PCB boards](http://galaxy22.dyndns.org/dtv/v2/hardware/):
    [Keyboard/Floppy/Joystick/Video/Audio/Userport solder
    points](:Image:C64DTV_solderpoints.jpg "wikilink") picture. If you
    want to change the casing: [bottom side solder
    points](:Image:C64DTV_solderpoints_bottom.jpg "wikilink") are more
    accessible.
    -   Make sure you patch the kernal with [kernal
        patcher](Kernalpatcher "wikilink") before doing the userport mod
        since the standard kernal sets PAL/NTSC mode depending on the
        userport's wiring.
    -   Joystick connector pinout: JoyX0=Up=Pin1, JoyX1=Down=Pin2,
        JoyX2=Left=Pin3, JoyX3=Right=Pin4, JoyX4=Fire=Pin6, Gnd=Pin8,
        +5V=~Bat+=Pin7
    -   Bat+ is the batteries' positive pin so it's about 6 Volts with
        normal batteries (too much for a keyboard!) and about 4,8 Volts
        with rechargeable batteries
    -   [IEC/PS2/S-Video/Joystick
        pinout](:Image:Connectors.png "wikilink")

Case Mods
---------

-   [DTV64 Micro
    Tower](http://www.64hdd.com/projects/hardware/c64-dtv64.html): Case
    mod, with all the trimmings.
-   [DTV Keyboard
    Console](http://www.64hdd.com/projects/hardware/c64-dtv64kb.html): A
    slim keyboard casemod.
-   [Picodore
    64](http://www.picobay.com/projects/2007/01/the-picodore-64-a-commodore-64-pda.html):
    Miniature DTV laptop
-   [Various case
    mods](http://galaxy22.dyndns.org/dtv/common/hacks/index.html)
-   [Expansion bus mod](http://home.earthlink.net/~dtvii/)
-   [DTV Yellow Box](http://www.nightfallcrew.com/?p=426): DTV in a Box

Additional hardware
-------------------

-   [PS2 Mouse on the
    DTV](http://galaxy22.dyndns.org/dtv/common/ps2mouse/index.html):
    Adding a PS/2 Mouse to the DTV

also see [Peripherals and Add-ons](Peripherals_and_Add-ons "wikilink")
