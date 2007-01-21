---
title: DTVLoad Details
permalink: DTVLoad_Details
---

#### DTVLoad

DTVLoad was written by Adrian Gonzalez, and used extensively by the team
during the Hummer's software & hardware development.

#### Initiating DTVLoad

*On a Hummer with unmodifed ROM:*

Enter Basic Mode by holding down TAB, LEFT or RIGHT CTRL during startup
brings up the DTVLOAD/Basic screen:

`**** COMMODORE 64 BASIC V2 ****`  
`64K RAM SYSTEM 38911 BASIC BYTES FREE`  
`WAITING FOR DTVLOAD. ANY KEY FOR BASIC`

If you release the key within a certain period, the unit will wait for a
DTVLOAD transfer. Any subsequent keypress will drop into Basic Mode. So
leaving the above keys pressed during startup will (generally) result in
the READY prompt and flashing cursor which denote Basic Mode.

Programs loads were triggered by resetting the Hummer.

#### PC software

A DTVLoad session also requires a 'server' program running on the PC,
also the source of the stored C64 executables. More info to come...

#### DTVLoad Cable spec

-   Remove the smd device R7
-   Remove the smd devices at U3-U7

On a Female DB25 parallel port, connect as follows

-   GND to Hummer GND
-   Data0 - User0
-   Data1 - User1
-   Data2 - User2
-   Data3 - User3
-   Data4 - User4
-   Data5 - User5
-   Data6 - User6
-   Data7 - User7
-   BUSY - PA2 (Hummer board, back, TP10)
-   STROBE - ATN IN (Chip-side of R7)

#### Caveats

The DTVLoad code is not included in the modifed kernals normally used to
replace the factory kernal. Those modded kernal are based on the PAL DTV
V2, and are the standard fix for:

-   The BASIC “SAVE bug”
-   Hardcoding NTSC timing code (stock kernal examines userport to
    specify video timing. The userport bit state is set by resistors at
    the factory.)
-   A flash-load routine bug

