---
title: Keyboard Twister
permalink: Keyboard_Twister
---

About
-----

Shadowolf's keyboard twister is an ATtiny45-based hardware solution to
fix some issues in the DTV's keyboard emulation as well as mapping
german keyboard layout.

Features
--------

-   Map german keys properly
    -   Mapping to other keyboards is also possible. Please contribute
        any other mappings!
-   Fix switched SHIFTs
-   Fix F7
-   Fix powerup problem (keyboard does not work properly on first
    startup but only after reset)

TODO:

-   “Twister should send $f4 to keyboard (and drop the resulting $fa)
    whenever it gets $aa. Zee used a PIC that did this to fix a PS/2
    'compatible' keyboard (that I happened to own too).” - Nojoopa
-   Keeping some mapped keys pressed is visible on DTV side as multiple
    keypresses. This does not matter in BASIC but is annoying in games
    (e.g., in Elite decelerating using “/” is very slow).
-   Add macro functionality for F9-F12. In record mode (Win+Fx?), write
    keycodes of keys pressed to EEPROM.

Contact
-------

-   [Forum64.de thread:
    “Keyboard-Twister”](http://www.forum64.de/wbb3/index.php?page=Thread&threadID=16194)
-   [Forum64.de thread: “Keyboard Twister
    ng”](http://www.forum64.de/wbb3/index.php?page=Thread&threadID=31605)
    (unfinished)

Download
--------

**[Source code and binaries](Media:Keyboard_twister.zip "wikilink")**

Hardware
--------

<img src="KeyboardTwister.png" title="KeyboardTwister.png" alt="KeyboardTwister.png" width="300" />

-   CKDIV8 has to be disabled (unprogrammed/1) so that the ATtiny runs
    at 8MHz.
-   PC\_CLOCK/PC\_DATA are C64DTV side.

Programming an ATtiny45 with
[AVRDUDE](http://savannah.nongnu.org/projects/avrdude/):  
`avrdude -p t45 -U flash:w:scancon.hex:i -U lfuse:w:0xE2:m`

OSX with AVR-USB500 SMD:  
`avrdude -c stk500v2 -P /dev/tty.usbserial-A20008Du -p ATtiny45 -U flash:w:keyboardtwister.hex:i -U lfuse:w:0xE2:m`

There have been
[reports](http://www.forum64.de/wbb3/index.php?page=Thread&postID=229143#post229143)
of Keyboard Twister behaving weird because of the two 5k1 resistors.
Better leave them away - in circuit programming of the ATtiny can still
be done (detach keyboard for programming then).
