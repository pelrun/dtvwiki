---
title: Joystick port
permalink: Joystick_port
---

The Commodore 64 has two joystick ports, also known as control ports,
which allow connection of various input devices, such as joysticks,
paddles, etc. The analog inputs are not implemented on the DTV, and
hence it is only possible to use digital inputs, such as normal
C64/Atari-style joysticks. The connectors used are DB-9, with a male
connector on the c64 (or DTV) and a female connector on the joystick.
The pin-out for the male connector is shown in the following figure:

![Pin-out for the DB-9 control
port](Joy-male_160.gif "Pin-out for the DB-9 control port")

As is the case with a standard C64, joysticks are wired into the
keyboard matrix, and are hence intimately connected with the keyboard
scanning. This means that wiggling joystick 1 while at the BASIC prompt
will affect the keyboard scanning, and hence most games that use only
one joystick use port 2.

**Port 2** is also the one that is connected to the joystick in the DTV.
Again, emulating the C64 configuration, this port goes to port A of the
first 6526 CIA (lines PA0 to PA4), found at address $DC00. This is also
the column driver for the keyboard matrix, and is normally configured as
outputs (by writing $FF to $DC02) in the keyboard scanning routine.

**Port 1** is brought out of the ASIC in the form of the lines used for
the extra buttons on the DTV: A, B, C, D and the right 'fire' button
(wired to JoyB4/Fire/PB4, JoyB0/Up/PB0, JoyB1/Down/PB1, JoyB2/Left/PB2,
JoyB3/Right/PB3, respectively - note the right button is *not* joy1
fire). This goes to port B of the first 6526 CIA, and is therefore found
at address $DC01. Port B is also used for row inputs from the keyboard
and is normally configured as input (by writing $00 to $DC03) in the
kernal keyboard scanning routine. However, *contrary* to the original
C64, the extra buttons on the DTV are not using ground as common, but
rather use PA0 ($DC00 bit 0) as common. This means that the extra
buttons emulate keys (F1, DEL, RETURN, CRSR RIGHT, F7) rather than
joystick lines in the DTV's default wiring (unless PA0 is forced to GND
by pushing the built-in joy2 up - then the extra buttons behave just as
known from a C64 joy1). Keep this in mind if you [fix
games](Fixing_Games_for_the_DTV "wikilink").

As a concluding remark: When modding for connecting an extra joystick,
you probably will use ground as common, so joystick port 1 will behave
exactly as normal for a true C64.

Connecting Joystick Port 2
--------------------------

The images below show where to connect joystick port 2 on the DTV PAL v3
board.

![Joystick Port 2 connections (solder
side)](DTV_v3_joy2_solder.jpg "Joystick Port 2 connections (solder side)")

![Joystick Port 2 connections (component
side)](DTV_v3_joy2_comp.jpg "Joystick Port 2 connections (component side)")

| DTV             | Joystick     | DB-9 pin | $DC00 bit |
|-----------------|--------------|----------|-----------|
| Joystick Up     | Port 2 Up    | 1        | 0 (PA0)   |
| Joystick Down   | Port 2 Down  | 2        | 1 (PA1)   |
| Joystick Left   | Port 2 Left  | 3        | 2 (PA2)   |
| Joystick Right  | Port 2 Right | 4        | 3 (PA3)   |
| Left Firebutton | Port 2 Fire  | 6        | 4 (PA4)   |
|                 | Common (GND) | 8        | -         |

Connecting Joystick Port 1
--------------------------

The images below show where to connect joystick port 1 on the DTV PAL v3
board.

![Joystick Port 1 connections (solder
side)](DTV_v3_joy1_solder.jpg "Joystick Port 1 connections (solder side)")

![Joystick Port 1 connections (component
side)](DTV_v3_joy1_comp.jpg "Joystick Port 1 connections (component side)")

| DTV              | Joystick     | DB-9 pin | $DC01 bit | Key        |
|------------------|--------------|----------|-----------|------------|
| B                | Port 1 Up    | 1        | 0 (PB0)   | DEL        |
| C                | Port 1 Down  | 2        | 1 (PB1)   | RETURN     |
| D                | Port 1 Left  | 3        | 2 (PB2)   | CRSR RIGHT |
| Right firebutton | Port 1 Right | 4        | 3 (PB3)   | F7         |
| A                | Port 1 Fire  | 6        | 4 (PB4)   | F1         |
|                  | Common (GND) | 8        | -         |            |

Note that the 5 extra buttons on the DTV use PA0 as common, not ground
(as should be used for a joystick connector). This also means that the
buttons A, B, C, D and right fire actually simulate keyboard presses
rather than joystick events. See
[Keyboard\_port\#Extra\_Buttons](Keyboard_port#Extra_Buttons "wikilink")
for more details. This does *not* apply for joysticks connected
externally if you use ground as common.
