---
title: IEC port
permalink: IEC_port
---

Connection
----------

The DTV allows connection of IEC devices, such as 1541 (and similar)
floppy drives. This is currently the easiest way of getting software on
to the DTV (although transferring by cable is possible, this requires
special software to be loaded on the DTV first).

The IEC bus is a 5V open collector bus consisting of a clock and a data
line, as well as an ATN (attention) signal. Apart from this a ground
connection is needed. The connections on the PAL DTV (v3) are listed
below.

![IEC connections on PAL v3 DTV (solder
side)](DTV_v3_IEC_solder.jpg "IEC connections on PAL v3 DTV (solder side)")

![IEC connections on PAL v3 DTV (component
side)](DTV_v3_IEC_comp.jpg "IEC connections on PAL v3 DTV (component side)")

-   ATN is found on TP1 on the solder side, or right next to R13 on the
    component side.
-   CLK is found on TP8 on the solder side, the pad between R35 and R47
    on the component side.
-   DATA is found on TP9 on the solder side, the pad between R47 and R48
    on the component side.

These pads are covered by the solder stop mask. Carefully scrape off the
(e.g. with an X-acto knife) lacquer layer to expose the copper
underneath before soldering.

IEC devices use a 6-pin DIN connector (60° pin separation and a center
pin - not a mini-DIN). The pin-out listed below shows the female
connector (that should be mounted in the DTV), from the outside (i.e.
*not* the solder side).

![The IEC connector (6-pin DIN Female)
pin-out](IEC-connector.png "The IEC connector (6-pin DIN Female) pin-out")

-   Pin 1 - SRQ is not used on the DTV.
-   Pin 6 - Listed as N.C. (not connected) is in fact connected to Reset
    on the original C64.

