---
title: DTV Version 1
permalink: DTV_Version_1
---

Specifications
==============

-   DTV1 core.
-   128 KiB RAM
-   2 MiB ROM
-   NTSC output only.
-   6502 emulation with few or no undocumented opcodes.

External Ports
--------------

-   Composite video
-   Audio

Internal Ports
--------------

These are only available by soldering.

-   PS/2 port (emulates the C64 keyboard matrix)
-   IEC port for connection of peripherals.
-   S-Video
-   Joystick port one (only left, right, down, and fire connections
    available, no up.)

Built-in Games
--------------

-   Bull Riding (Event from World Games, Epyx, 1986)
-   Championship Wrestling (Epyx, 1986)
-   Cyberdyne Warrior (Hewson, 1989)
-   Cybernoid (Cybernoid: The Fighting Machine, Hewson, 1988)
-   Cybernoid II (Cybernoid II: The Revenge, Hewson, 1988)
-   Eliminator (Hewson, 1988), Exolon (Hewson, 1987)
-   Firelord (Hewson, 1986)
-   Flying Disk (Event from California Games, Epyx, 1987)
-   Gateway to Apshai (Epyx, 1983)
-   Impossible Mission (Epyx, 1984)
-   Impossible Mission 2 (Epyx, 1988)
-   Jumpman Junior (Epyx, 1983)
-   Paradroid (Hewson, 1985)
-   Pitstop (Epyx, 1983)
-   Pitstop 2 (Epyx, 1984)
-   Ranarama (Rana Rama, Hewson, 1984)
-   Silicon Warrior (Epyx, 1984)
-   Speedball (Image Works, 1989)
-   Summer Games (Epyx, 1984)
-   Super Cycle (Epyx, 1986)
-   Sumo (Event from World Games, Epyx, 1986)
-   Surfing (Event from California Games, Epyx, 1987)
-   Sword of Fargoal (Epyx, 1983)
-   Tower Toppler (Hewson, 1987)
-   Uridium (Hewson, 1986)
-   Winter Games (Epyx, 1985)
-   World Karate Champion A (World Karate Championship, Epyx, 1986)
-   World Karate Champion B (World Karate Championship, Epyx, 1986)
-   Zynaps (Hewson, 1987)

(list of games from
<http://www.lemon64.com/forum/viewtopic.php?p=173248#173248> )

In addition, the following games are built in as 'easter eggs':

-   “Minima Reloaded” by Robin Harbron (AKA Macbeth/PSW) (4k compo
    winner 2003)
-   “Splatform” by Robin Harbron (AKA Macbeth/PSW) (1k compo
    winner 2002)
-   “dWcave” by Adrian Gonzalez (AKA dW/Style)
-   “Tinyrinth” by Mark Seelye (AKA Burning Horizon/FTA)
-   “Astrostorm” by Vanja Utne (AKA Mermaid/Creators)
-   “Cliff Diving” by Darren Foulds (AKA Shroom/PSW)

Software Problems
-----------------

At LEAST the 041101 revision has a ROM bug regarding the control in
“Sword of Fargoal”. A battle is automatically fled in the same direction
it was entered TO (i.e. west-to-monster will flee west) after one turn,
making battles take a long time and require lots of re-entering. If a
wall is present in the tile the player will flee TO, and in both of the
adjacent tiles in the 8 cardinal directions, the game will get stuck in
an infinite loop displaying the monster name and playing the 'entered
battle' tune. The original c64 version does not have this problem.

This problem was fixed for the PAL (dtv2/3, released by Toy Lobster)
version. The bug was created when the code was being modified to account
for the lack of keyboard on the DTV, and wasn't discovered until the
NTSC DTV had gone into production.

Revisions
---------

The batch number is “hotstamped” into the plastic on the bottom of the
joystick just beside the battery compartment, as shown below. It is not
identical to the batch number stamped onto the retail packaging. The
assumed interpretation is YYMMDD.

![DTV batch number](DTV_v3_batchnum.jpg "DTV batch number")

The DTV1 had a number of things which varied considerably between (and
even within) different batches:

-   The PCB was either a C64A version with 3 jumper wires attached to
    it, or a C64A1 version with those wires integrated into the pcb as
    traces.
-   The direction pad contacts were either made of yellow rubber
    (generally regarded as superior) or grey rubber.
-   The external case was either labeled C=64 or C= Commodore 64 (the
    latter, as implied by a post by Jeri, were a run of early/prototype
    cases and the final lot of manufactured cases all read C=64). Oddly
    the 'C= Commodore 64' cases seem to appear in the LATER batches,
    perhaps the manufacturer ran out of the 'real' cases and had lots of
    the 'useless prototype cases' lying around and used those?

| Batch  | PCB REV     | Core       | ROM CRC    | Dir pad contact | PostPCB wires | Label           | Other                      | Reporter                     |
|--------|-------------|------------|------------|-----------------|---------------|-----------------|----------------------------|------------------------------|
| 041028 | not checked | not tested | not tested | Yellow          | unknown       | C=64            |                            | (CORE321)                    |
| 041029 | C64A        | not tested | not tested | Grey            | 3             | C=64            | early release unit         | (dennis from AroundMyRoom)   |
| 041030 | not checked | not tested | not tested | Grey            | unknown       | C=64            |                            | (CORE321)                    |
| 041030 | not checked | not tested | not tested | unknown         | unknown       | C=64            |                            | (MacbthPSW)                  |
| 041101 | C64A1?      | not tested | not tested | Yellow          | 0             | C=64            |                            | (Lord Nightmare)             |
| 041102 | C64A?       | not tested | not tested | unknown         | 3             | C=64            |                            | (Junoman)                    |
| 041104 | C64A1       | tested?    | 0x???????? | Yellow          | 0             | C=64            |                            | (Jim Brain)                  |
| 041104 | not checked | not tested | not tested | unknown         | unknown       | C=64            |                            | (MacbthPSW)                  |
| 041105 | C64A1       | not tested | not tested | unknown         | unknown       | C= Commodore 64 |                            | (JoeCommod) (June Tate-Gans) |
| 041106 | not checked | not tested | not tested | Grey            | unknown       | C= Commodore 64 |                            | (CORE321)                    |
| 041108 | not checked | not tested | not tested | unknown         | unknown       | C= Commodore 64 | buzzy SID, returned        | (laner)                      |
| 041109 | not checked | not tested | not tested | unknown         | unknown       | C= Commodore 64 | buzzy SID, returned        | (laner)                      |
| 041113 | not checked | not tested | not tested | unknown         | unknown       | C=64            | noise issues, buzzy sound? | (MacbthPSW)                  |
| 041114 | C64A1       | not tested | not tested | Yellow          | unknown       | C=64            |                            | (PiXEL8)                     |
| 041115 | C64A1       | not tested | not tested | Grey            | unknown       | C= Commodore 64 | sold in europe             | (HBEP)                       |

External Links
--------------

-   [DTV v1
    Schematic](http://galaxy22.dyndns.org/dtv/common/tech/dtv-schematic.png)
-   [Cleaned up DTV v1 Schematic, by Jim
    Brain](http://www.jbrain.com/vicug/gallery/c64dtv/C64DTV_NTSC_Schematic)

