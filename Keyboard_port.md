---
title: Keyboard port
permalink: Keyboard_port
---

Connection
----------

The keyboard port on the DTV uses the AT or PS/2 protocol consisting of
a two-signal (clock and data) open-collector 5V bus. Apart from the two
signals, ground and 5V must be supplied to the keyboard connector as
well. Under most circumstances the easiest way of supplying 5V for the
keyboard is by replacing the battery of the DTV with a 5V DC power
supply, which will also feed the keyboard. Although the nominal voltage
of the batteries in the DTV would be 6V, it runs perfectly fine at 5V.
The inputs on the ASIC are 5V tolerant, so running the keyboard at 5V
poses no problem here.

![PAL DTV v3 component side keyboard
connections](DTV_v3_keyboard_comp.jpg "PAL DTV v3 component side keyboard connections")

![PAL DTV v3 solder side keyboard
connections](DTV_v3_keyboard_solder.jpg "PAL DTV v3 solder side keyboard connections")

The above images show where to connect the keyboard on PAL DTV v2/v3.
The Clock-line is marked with TP3 on the solder side, and corresponds to
one side of the resistor R3 (opposite the text R3 on the silk-screen).
The Data-line is TP4 and is connected to one side of R4 (again, opposite
the text R4 on the silk-screen).

The ground connection can be found in several places on the DTV board. A
suitable point is shown in each of the pictures above.

![PS/2 keyboard connector
pin-out](PS2-connector.png "PS/2 keyboard connector pin-out")

![AT keyboard connector
pin-out](AT-connector.png "AT keyboard connector pin-out")

The above images show the pin-outs for the possible connectors for
hooking up either a PS/2 or an AT keyboard to the DTV. The shown
connectors are both female types for mounting in the DTV (the male plug
is on the keyboard).

Emulation
---------

The keyboard port on the DTV emulates the Commodore 64 keyboard matrix
by decoding the scan codes from the keyboard and converting these into
C64 keyboard matrix positions which are simulated on the 6526 CIA ports.

The following keys on the keyboard map to C64 keys:

-   Pause: RESTORE
-   ESC: RUN/STOP
-   Tab or Ctrl: CTRL
-   Left Alt: C=
-   Pos1: Clr/Home
-   Delete: Inst/Del
-   Page up: (Pound)
-   Page down: (Arrow up)
-   SHIFT+Page down: (Pi)
-   Alt Ret: C= + SHIFT

Some keyboard positions are wired into the keyboard matrix in a way that
actually generates multiple keys. For instance the C64 only has 2
cursor-keys and shift is used to change the direction. When a PC
keyboard is connected to the DTV, pressing the up arrow-key results in
both shift and CRSR-U/D, thus resulting in the cursor moving up, as
expected. In a similar fashion the even F2, F4, F6 and F8 generate
Shift-F1, Shift-F3, Shift-F5 and Shift-F7, as they would on a Commodore
C64.

Furthermore some key positions are remapped in order to utilize American
keyboard layout and convert this into a C64 equivalent. This affects the
layout of some special characters, which are in different positions on
the C64 keyboard layout and the American one.

The implementation found in the DTV has the following flaws:

-   The F7 key does not work (DTV expects 0xE0 0x03 instead of 0x83, see
    [nojoopa's
    findings](http://jledger.proboards19.com/index.cgi?board=dtvhacking&action=display&thread=1171796543)).
    -   **If you intend to get rid of the original DTV2 casing, build
        [Keyboard Twister](Keyboard_Twister "wikilink") or add an extra
        F7 key as a button connecting Joy2 Up and Joy1 Right. Otherwise
        you will not have an F7 key.**
-   The two shift keys are reversed
-   On initial powerup, the DTV's PS/2 hardware gets confused by startup
    byte sent by keyboard.

It is possible to change the keyboard mapping in software and hardware.

-   [MadModder's
    KEYREMAP.PRG](http://jledger.proboards19.com/index.cgi?board=dtvhacking&action=display&thread=1132865860) -
    DTV kernal patch. Only works with software using kernal keyscan
    routines (= only very few applications such as BASIC programs). Does
    not fix startup or F7 issues.
-   [Keyboard Twister](Keyboard_Twister "wikilink") - ATTiny45-based
    hardware solution also fixing startup and F7 issue

Extra Buttons
-------------

The extra buttons A, B, C, D on the base of the DTV unit as well as the
right fire button are actually wired into the keyboard matrix.
Conveniently the extra buttons on the DTV are also the lines for control
port 1, allowing the connection of a second joystick. For the A, B, C, D
and right fire buttons, these do not use a common ground, but instead
are connected to row 0 of the keyboard matrix (e.g. bit0 on $DC00). The
buttons therefore generate the following keypresses:

| DTV button | bit number | keypress |
|------------|------------|----------|
| B          | 0          | DELETE   |
| C          | 1          | RETURN   |
| D          | 2          | CRSR RT  |
| right btn  | 3          | F7       |
| A          | 4          | F1       |

Also see [Joystick port](Joystick_port "wikilink").

Keyboard Matrix
---------------

The mapping from an AT or a PS/2 keyboard to the matrix is not a key for
key direct mapping. Keys get mapped differently depending on currently
depressed modifiers (shift, ctrl, alt). The mapping is hardcoded for
US-English keyboards and thus causes some strangeness in the mapping for
other variants.

### Original C64 mapping

|               |         | $dc01 (read) |
|---------------|---------|--------------|
|               |         | Bit 7        |
| $dc00 (write) | Bit 7   | STOP         |
| Bit 6         | /       | ^            |
| Bit 5         | ,       | @            |
| Bit 4         | N       | O            |
| Bit 3         | V       | U            |
| Bit 2         | X       | T            |
| Bit 1         | LSHIFT  | E            |
| Bit 0         | CRSR DN | F5           |


