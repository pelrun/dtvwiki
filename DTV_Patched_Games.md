---
title: DTV Patched Games
permalink: DTV_Patched_Games
---

This is a list of games that has been patched for the [DTV Version
2/3](DTV_Version_2/3 "wikilink"). See [Userport Patched
Games](Userport_Patched_Games "wikilink") for games patched for the
[Hummer](Hummer "wikilink").

Actual repository for games is [Spiff's
repository](http://symlink.dk/nostalgia/dtv/fixed/). This list is
intended to document changes both from a gamer's and programmer's point
of view. See [Fixing Games for the
DTV](Fixing_Games_for_the_DTV "wikilink") for a programmer's guide.

Note that some files in the repository extend above $CFFF and cannot be
loaded with normal floppy LOAD. See
[here](DTV_Version_2/3#Some_games_from_Spiff.27s_Repository_fail_to_run_properly._Why_is_that.3F "wikilink")
for details.

Collections
===========

Ebster's collection
-------------------

**[Download](Media:ebster_dtv_collection_1.zip "wikilink")**. Spiff ZIP
format (suitable for [DTVFSEdit](DTVFSEdit "wikilink") and
[dtvmkfs](dtvmkfs "wikilink")). Contains a lot of freezed games. See
[Forum64 thread](http://www.forum64.de/wbb2/thread.php?threadid=15580)
for a list of games included. Choplifter was broken and has been
replaced by a version by bencao74. The menu has been replaced by
[DTVSlimIntro](DTVSlimIntro "wikilink"). [DTVTrans](DTVTrans "wikilink")
has been added. Bubble Bobble has been replaced with the 2p fixed
version. Can be flashed easily using [DTVTrans](DTVTrans "wikilink")
sync. No kernal changes needed. Most of these files have been included
in Spiff's repository as individual archives.

Games
=====

Armalyte
--------

Original release by Remember. Patched for DTV by
[1570](User%3A1570 "wikilink").

-   Changed keyboard control to use DTV buttons
-   Fixed graphics
-   Added autofire
-   Level loading from RAM

See [Fixing Games for the DTV Example:
Armalyte](Fixing_Games_for_the_DTV_Example:_Armalyte "wikilink").

Patched in VICE. Program file generated using
[VSFReanimator](VSFReanimator "wikilink"). Tested in
[VICEplus](http://viceplus.sourceforge.net/).

Beamrider
---------

Original release by Cyberpunx. Patched for DTV by
[1570](User%3A1570 "wikilink").

-   Changed keyboard control to use DTV buttons
-   Added difficulty level 4
-   Use joystick 2 for all players

`# Replace keys 1-4 with buttons A-D`  
`> 8983 a0`  
`> 8988 80`  
`> 898d 88`  
`> 8992 90`  
  
`#Instructions`  
`> 8931 41 5c 44`  
`> 8966 41 5c 44`  
  
`#Use Joy2 instead of Joy1`  
`> 8ab4 02`  
`> 8ab9 03`  
`> 8abc 01`  
`> 8abf 00`  
  
`#Difficulty up to 4`  
`a 88f4 cpx #$04`  
  
`#Change difficuly/level tab`  
`a 823a lda $c200,x`  
`> c200 00 07 0e 13`  
  
`Level number: $0800`  
  
`#Go to start screen after game over`  
`a 87c3 jmp $8009`  
  
`#Use Joy2 for all players`  
`a 8aa0 jmp $8ab1`  
`nop`

Patched in VICE. Program file generated using
[VSFReanimator](VSFReanimator "wikilink").

Bruce Lee
---------

Original release by Remember. Patched for DTV by
[1570](User%3A1570 "wikilink").

-   Changed keyboard control to use DTV buttons
-   Fixed title screen (did not respond to input even with keyboard
    attached)
-   Swapped joysticks

`# DTV keyscan problem`  
`Routine at $13ae writes to $DC01, reads from $DC00`  
`> 13b1 02`  
`> 13bb 00`  
`> 13be 01`  
  
`$010d/PA used by keyscan routine at $142c :-(`  
`a 142e jmp $143a`  
`nop`  
`nop`  
  
`# Swap joysticks`  
`a 16c1 tax`  
`eor #$01`  
  
`> 0f6 00`  
`> 16a3 00`  
`> 16aa 01`  
`> 16db 00`  
`> 16e1 01`  
`> 19be 00`  
  
`a 572d sta $99`  
`lda $03`  
`eor #$01`  
`tax`  
`jmp $16f8`  
  
`a 16f4 jmp $572d`  
  
`# Pause - Button D`  
`> 148c fe fb`  
  
`# Change title screen keys to DTV buttons`

Patched in VICE. Program file generated using
[VSFReanimator](VSFReanimator "wikilink").

Bubble Bobble
-------------

Original release by Remember. Patched for DTV by
[1570](User%3A1570 "wikilink").

-   Changed keyboard control to use DTV buttons (RFire+D: Levelskip,
    B+C: Pause)
-   Changed trainer control to use DTV buttons

`Remember:`  
`$5402 - trainer keyscan`  
  
`a 5405 cmp #$88`  
  
`a 5409 cmp #$1d`  
  
`a 540d cmp #$0d`  
  
`a 5411 cmp #$14`  
  
`a 5415 cmp #$85`  
  
`> 060c 20 20 20 55 13 05 20 41 2f 46 31 20 42 2f 44 45 4c 20 14 0f 20 13 05 0c 05 03 14 20 20 20 20 20 20`  
`> 0636 20 20 20 20 43 2f 52 45 54 55 52 4e 20 44 2f 52 49 47 48 54 20 14 0f 20 20 20 20 20 20`  
`> 06b0 20 20 20 20 52 46 09 12 05 2f 46 37 20 14 0f 20 13 14 01 12 14 20 20 20 20 20`  
  
`> a0d 0c`  
`a 3fcd rts`  
  
`a 7eb6 jsr $3fc3`  
`LDY #$FF`  
`STY $DC00`  
`LDA $DC01`  
`EOR #$FF`  
`DEY`  
`STY $DC00`  
`ORA $DC01`  
`INY`  
`STY $DC00`  
`EOR #$FF`  
`AND $DC01`  
`EOR #$FF`  
`cmp #$f3   /* RFire + D / F7 + Right */`  
`bne *+5`  
`inc $21`  
`rts`  
`cmp #$fc   /* B + C / Ret + Del */`  
`rts`  
  
`a f0cf cmp #$ef`  
  
`a f0d3 cmp #$fe`

Patched in VICE. Program file generated using
[VSFReanimator](VSFReanimator "wikilink").

Druid
-----

Original release by Remember. Patched for DTV by
[1570](User%3A1570 "wikilink").

See [Fixing Games for the DTV Example:
Druid](Fixing_Games_for_the_DTV_Example:_Druid "wikilink").

-   Changed keyboard control to use DTV buttons
    -   A missile, RFire golem ctrl, A+RF Pause, B key, C invis, D
        golem, RF+D chaos

Patched in VICE. Program file generated using
[VSFReanimator](VSFReanimator "wikilink").

Hard'n'Heavy
------------

Original release by Remember. Patched for DTV by
[1570](User%3A1570 "wikilink").

`; Use Joy1/2-safe keyscan`  
`a 76c`  
`SEI`  
`LDA #$FF`  
`STA $DC02`  
`LDA #$FF`  
`STA $DC03`  
`LDA #$FF`  
`STA $DC01`  
`JMP $9000`  
  
`a 9000`  
`LDX #$FF`  
`STX $DC00`  
`LDA $DC01`  
`CMP $DC01`  
`BNE $9005`  
`EOR #$FF`  
`DEX`  
`STX $DC00`  
`ORA $DC01`  
`INX`  
`STX $DC00`  
`EOR #$FF`  
`AND $DC01`  
`EOR #$FF`  
`JMP $077F`  
  
`; Use RFire for selecting extras`  
`> 78d f7`  
  
`; Levelskip => B+C`  
`> 0352 fc`  
`> 034a fe`

Patched in VICE. Program file generated using
[VSFReanimator](VSFReanimator "wikilink") and dtvDexomizer.

Mayhem in Monsterland
---------------------

Original release by Warriors of the Wasteland. Patched for DTV by tlr
and [1570](User%3A1570 "wikilink").

-   Changed keyboard control to use DTV buttons
    -   RFire+D: Levelskip
    -   RFire+A/B: Powerup/Invincibility
-   Fixed graphics
-   Level loading from RAM

See [Fixing Games for the DTV Example:
Mayhem](Fixing_Games_for_the_DTV_Example:_Mayhem "wikilink").

Patched in VICE. Program file generated using
[VSFReanimator](VSFReanimator "wikilink"). Tested in
[VICEplus](http://viceplus.sourceforge.net/).

North and South
---------------

Original release by Remember. Patched for DTV by
[1570](User%3A1570 "wikilink").

-   Changed keyboard control to use DTV buttons
    -   B/C in battle to switch units
-   Level loading from RAM

`08fc/093a (endbreak 0973)  loader, filename at 08f9`  
  
`undump `“`load.vsf`”  
`break 093a`  
`x`  
`bank ram`  
`> 08f9 30 32`  
`f 0c00 fff0 56`  
`break 0973`  
`x`  
  
`Put patched DTV flash loader (see Armalyte/Mayhem) to 0945`  
  
`884d/8891 to 893d keyscan routine`  
`RUN/STOP  Unit North   88ea  (ACC=4)`  
`F5        Unit South   889b  (ACC=5)`

Patched in VICE. Program file generated using
[VSFReanimator](VSFReanimator "wikilink"). Tested in
[VICEplus](http://viceplus.sourceforge.net/).

Rally Speedway
--------------

Original release unknown. Patched for DTV by
[1570](User%3A1570 "wikilink").

-   Changed keyboard control to use DTV buttons
    -   Editor:
    -   Select/Back - A/F1
    -   Choose Track - B/Del
    -   Fill - C/Return
    -   Back to menu - RFire/F7
-   Swapped joysticks

`# Joysticks`  
`> 658b 00`  
`> 659e 01`  
  
`# Editor`  
`SPACE - Select/Back - ButA/F1: > 6cf5 04  > 6cdf 04`  
`F3 - Choose Track - ButB/DEL:  > 6d78 00  > 6d52 00`  
`F1 - Fill - ButC/RETURN:       > 6d35 01  > 6d03 01`  
`F7 - To Menu - RFire`

Patched in VICE. Program file generated using
[VSFReanimator](VSFReanimator "wikilink").

Skier
-----

Commodore/HAL version.

`Ingame (use Joy2 instead of Joy1)`  
`> f1eb 01`  
`> f1ee 00`  
  
`Start screen (A/B one/two players)`  
`> e592 01`  
`> e430 20 01`  
`> e43f 20 02`

The Sentinel
------------

Original release by Remember. Patched for DTV by
[1570](User%3A1570 "wikilink").

-   Changed controls.
-   Enabled skip+burst.
-   [Patch code](Media:sentinel-patch.zip "wikilink") (xa format)

Patched in VICEplus/x64dtv. Program file generated using
[VSFReanimator](VSFReanimator "wikilink") and dtvDexomizer.

Triple Tournament
-----------------

Patched for DTV by [1570](User%3A1570 "wikilink").

`$0a5d: one/two player check`  
`$0f89: speed/function keys check`  
`$0b24: space check`  
`$0b28: >=1 check`  
`$0b2c: <4 check`  
  
`# Return = C = One player`  
`> 0a5e 0d`  
`> 11e2 43`  
  
`# Cursor right = D = Two players`  
`> 0a62 1d`  
`> 120c 44`  
  
`# Return = C = All games`  
`> 0b25 0d`  
`> 145c 42 55 54 54 4f 4e 20 43 20`  
  
`# Select speed notice`  
`f 1174 11c2 20`  
`> 1174 50 52 45 53 53 20 41 20 4f 52 20 52 49 47 48 54 20 46 49 52 45 20 42 55 54 54 4f 4e 20 54 4f 20 53 45 4c 45 43 54 20 20 53 50 45 45 44`

Patched in VICEplus/x64dtv. Program file generated using
[VSFReanimator](VSFReanimator "wikilink") and dtvDexomizer.

Wicked
------

Original release by Illusion. Patched for DTV by
[1570](User%3A1570 "wikilink").

-   Fixed VICII IRQ issues on the DTV
-   Changed keyboard control to use DTV buttons
    -   Levelskip: D

`       .word $cda0`  
`       * = $cda0`  
  
`/*`  
`Fix VICII related issues`  
`> 218c 20 a0 cd`  
`> 21a1 20 a0 cd`  
`> 21e3 20 a0 cd`  
`> 22a5 20 a0 cd`  
`> 22cc 20 a0 cd`  
`> 2472 20 a0 cd`  
`> 2490 20 a0 cd`  
`> 3a5d 20 a0 cd`  
`> 16fc a9 7f`  
`> 2191 a9 7f`  
`> 21e8 a9 7f`  
`> 22aa a9 7f`  
`> 230d a9 7f`  
`> 2477 a9 7f`  
`> 2495 a9 7f`  
`> 259a a9 7f`  
`> 263c a9 7f`  
`> 3a62 a9 7f`  
  
`Levelskip on button D`  
`> 0b41 02`  
`> 093d 02`  
`> 0ada 02`  
`*/`  
  
`irqFix:`  
`        lda $d019`  
`        and #$01`  
`        beq noIrq`  
`        lda #$f1`  
`        rts`  
`noIrq:  lda #$70`  
`        rts`

Patched in VICE. Program file generated using
[VSFReanimator](VSFReanimator "wikilink") and dtvDexomizer.

Wizard
------

Patched for DTV by [1570](User%3A1570 "wikilink").

-   Fixed VICII IRQ issues on the DTV
-   Replaced level loader by high RAM loader (same procedure as Armalyte
    etc.)
    -   Wizard uses only LOAD so this is straighforward
        -   Convert D64 to flash image
        -   Load that to $010000 (in x64dtv: bank ram01, l..., bank cpu)
        -   Load RAM load routine to $5400
        -   Replace KERNAL LOAD calls at $8a8c and $6bb4 with jsr $5400
-   dump, [VSFReanimator](VSFReanimator "wikilink"), dtvDexomizer.

VICII IRQ fix: Replace

`.C:7cbf   AD 19 D0   LDA $D019`  
`.C:7cc2   10 08      BPL $7CCC`  
`.C:7cc4   29 04      AND #$04`  
`.C:7cc6   8D 19 D0   STA $D019`  
`.C:7cc9   4C E7 7C   JMP $7CE7`

by

`.C:7cbf   AD 19 D0   LDA $D019`  
`.C:7cc2   8D 19 D0   STA $D019`  
`.C:7cc5   29 04      AND #$04`  
`.C:7cc7   D0 1B      BNE $7CE4`  
`.C:7cc9   AD 0D DC   LDA $DC0D`  
`.C:7ccc   20 27 7B   JSR $7B27`  
`.C:7ccf   20 0F 8E   JSR $8E0F`  
`.C:7cd2   20 70 92   JSR $9270`  
`.C:7cd5   20 E7 92   JSR $92E7`  
`.C:7cd8   20 14 7B   JSR $7B14`  
`.C:7cdb   20 EA 93   JSR $93EA`  
`.C:7cde   20 DA 96   JSR $96DA`  
`.C:7ce1   4C 6A 7D   JMP $7D6A`  
`.C:7ce4   AD 1E D0   LDA $D01E`  
`.C:7ce7   F0 E0      BEQ $7CC9`  
`.C:7ce9   EA         NOP`
