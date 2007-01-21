---
title: Fixing Games for the DTV
permalink: Fixing_Games_for_the_DTV
---

Fixing DTV games guide
======================

This page provides some guidelines for patching games to run on the C64
DTV. Hopefully the instructions and arguments presented here will
explain why the patches are needed and give some pointers towards
getting started with the patching. This page replaces the original
[fixing DTV games guide](http://symlink.dk/nostalgia/dtv/fixed/guide/),
since this wiki allows much better collaboration, and has been updated
substantially since the original guide was written.

If you manage to patch a game, please have this game added to [the
repository](http://symlink.dk/nostalgia/dtv/fixed/).

Choose a game to fix
--------------------

Before patching a game for running on the DTV, you need to find out
which game you will be trying. Of course you will start out by selecting
your favorite game, but if this is your first attempt at patching, you
may want to look at a few different ones in order to find one that is
easier to get working. Also check [the
repository](http://symlink.dk/nostalgia/dtv/fixed/) and the [Requests
and Work-in-progress](Requests_and_Work-in-progress "wikilink") to make
sure you are not spending time re-inventing the wheel.

You may try to find **different versions** of the game and test if they
run on the DTV. For this you will probably want to have a floppy drive
(or other IEC-device) connected to your DTV, as well as a keyboard. In
some cases one version of a game might crash on the DTV, while another
works fine. Also, at this stage you may be able to find a version that
has less things to be fixed, such as one **without a
cracker/decruncher** screen to be removed etc.

Things to be fixed
------------------

Some games do not run on the DTV hardware, or have some flaws that
should be fixed before the games behave correctly on the DTV. Normally
this will be due to the hardware (ASIC) implementation in the DTV that
in some instances behaves slightly different that the original C64
hardware, although it is impressive how close it actually gets.

**VICII**: In particular some games and especially a lot of demos for
the C64 use special raster routines and fancy programming of the VIC
chip, which may result in graphical glitches on the DTV. This kind of
glitch can be extremely difficult or even impossible to fix, so start
out by finding a game that does not pose too many problems in this
respect.

**SID**: Some similar issues exist for the SID (sound) implementation on
the DTV. An important point is that the DTV does not implement SID
filters, and so the sound may sound considerably different than what one
might be used to from the C64. Fortunately a large percentage of games
do not use SID filters due to the fact that production differences on
the original C64 SIDs meant that the sound could appear quite
differently on different C64s when using filters. Again, the easiest is
to pick a game that works on the DTV hardware.

**Keyboard input**: In most cases the primary issue is games that
require keyboard input, either in-game, when starting the game (such as
select 1 or 2 players or select level), or for skipping the highscore
entry. Although this does not per se mean that the games cannot be
played on the DTV, it will require that a keyboard is hooked up to the
DTV in order for it to work. It is therefore desirable to patch any part
of the game that requires the keyboard to work with the joystick. You
can also use the five extra buttons found on the DTV (see [Joystick
port](Joystick_port "wikilink") for details). If this means that some
selections are not possible (due to limited keys), so be it, but it
would be preferable to be able to avoid the keyboard. A lot of games
seem to require selecting the number of players before starting,
normally by pressing either 1/2 or F1/F3. In this case it is desirable
that pressing the firebutton on joystick2 (i.e. the left button on the
DTV) results in a one-player game being started.

Similarly, a lot of times a C64 game has some loader/decruncher/cracker
screen that requires the use of the keyboard (normally by hitting the
space bar) to continue. Although it would certainly be possible to patch
this to react on the left button of the joystick, and this is an
acceptable solution, it is often just as easy to remove the screen
altogether.

**Floppy loading**: For games that consist of more than one file (known
as multi-part games), there may be a need to fix the loading routine in
order for the game to work on the DTV. Normally the game (if it is a
floppy-game) is loaded from device 8. The flash in the DTV can be
accessed using kernal routines (and LOAD) using device 1. It implements
random access semantics like a floppy.

Loading files from BASIC
------------------------

There are some things to keep in mind when packaging a fixed game.

-   Standard floppy load can only load up to $D000 (= 203 blocks). The
    DTV flash load routine, the fastload included in peiselulli's
    kernal, and [DTVTrans](DTVTrans "wikilink") (if started from flash)
    do not have this limitation.
-   A BASIC stub (one BASIC line containing only a SYS) makes things
    more comfortable

Some games shipped with the DTV are less than 203 blocks, but no BASIC
stub has been added at $0801, or they require some nonstandard PLA
configuration ($01). Therefore these games cannot be started from BASIC
(easily). When games are patched/hacked for the DTV, you should at least
add a BASIC stub.

As for the size, there is a consideration regarding the use of
compression. The flash filesystem on the DTV already has some simple
compression scheme, and the flash is rather large (compared to 1541
floppies), so the size is not as important in this respect. In many
cases it would be preferable to avoid the waiting time of decompressing
the game in memory. However, for example
[Exomizer](http://hem.bredband.net/magli143/exo/) decompression is
actually fairly quick, so this is preferred, especially if the game uses
RAM above $D000 but the compressed file is smaller than 203 blocks.

Software to use for fixing games
--------------------------------

Natively you can use [DTVMON](DTVMON "wikilink") to patch the game, it
resides either in hi-mem or in flash, so it does not interfere with the
game code. However, patching the game in
[VICEplus](DTV_Support_in_VICE "wikilink") and [transfering the code to
the DTV](HOWTO_:_transferring_programs_to_the_DTV "wikilink") as last
step is definitely recommended.

The monitor in VICE is very useful ([VICE monitor
docs](http://www.viceteam.org/vice_9.html)). ALT-H to activate. You can
do **disassembling** (*d*), **stepping** (*z*/*n* - trace into/skip
JSRs) through code and adding **breakpoints** (*break* - *x* to run
until breakpoint encountered). Adding **watchpoints** (*watch*) is very
useful - it allows you to set a “memory location breakpoint”, for either
read, write, or both. In this way, whenever an instruction tries to
access a particular piece (or range) of memory, execution stops. Since
I/O-devices are memory-mapped, this can also detect access to I/O.

For disassembling large parts of code you can use [**dxa
disassembler**](http://www.floodgap.com/retrotech/xa/) which is a
counterpart to the [**xa
assembler**](http://www.floodgap.com/retrotech/xa/), and produces output
that can be assembled back into the same code.

Let's start patching
--------------------

You start by trying to get a version **without a loader/decruncher**.
Look around on the net, and often you can find a lot of different
versions. Also try running each on the DTV. E.g., with *Montezuma's
Revenge* only some versions worked. Some crashed the DTV while others
sort of worked, but all the monsters were stationary.

If it is not possible to find a version that has no “cracker intro” or
decruncher, we will have to make this step ourselves. Basically there
are two ways to get rid of decrunchers: Manually find out when
decrunching has finished, or just freezing the game when it is running.

### Solution 1: Traditional way

Start by loading the program into memory, but don't run it yet. Set a
breakpoint at the beginning of the program before running. Then
single-step through the decruncher, or do it in small steps by looking
at what the code does and continously moving the breakpoint to after the
next loop. If there is an intro screen, you can also set a watch on
**$DC01**, since this normally **scans the keyboard** to detect when
space is pressed, to continue. Then set a breakpoint after space has
been detected and step from there.

When decrunching is done, you normally have the program extracted in
BASIC memory, e.g. starting at $0801, and often even with a nice BASIC
stub (sys addr). The last thing that happens, when the decruncher is
done, is usually a JMP to the address (which is normally the same as the
SYS address in the BASIC line).

Of course the decruncher itself needs to be some place. Often the small
decruncher routine will move itself either into the stack-page $100-$1FF
or into the screen memory $400-$4FF (this is what shows up as some
strange garbage characters that flickers while the decrunching happens).
So if you find a JMP to some address in the $800 range in either of
these two places, there is a good chance that this JMP is actually what
will happen when the program is fully decrunched in memory.

One thing to note: The decruncher may have put some data underneath the
kernel area ($E000-$FFFF), which is not as such a problem, but PRG files
that extends into the I/O region ($D000-$DFFF) will cause standard
floppy load to crash (loading from Flash in the C64DTV emulation should
be fine though). If we are lucky this is not the case, and our game
resides only beneath address $D000. Writing to the BASIC ROM area
($A000-$BFFF) is OK, because writing will always go to the memory
'behind' the ROM, even if the ROM is currently mapped in. If the game
stretches into the I/O region or kernel and we want to load the prg
using normal floppy load, we may need to do a few tricks, such as making
a small routine that copies some pages into this high area. A second
option is to crunch the final PRG with exomizer, which will correctly
map out BASIC, I/O and kernel while decrunching.

In any case, now is a good time to **save a bare PRG** of our work so
far. Do this from the monitor. We assume that nothing below address $800
is needed (since the PRG will normally be loaded from $801), and now
would be a great time to know exactly how far into memory the game
actually extends. Sometimes this information can be found from the
decruncher routine, and sometimes a bit of guess-work is needed. Save
the PRG file from the monitor with the command **s “filename.prg” 0 801
endaddr**. If you don't know the end address, or if is beneath the
BASIC, I/O or kernel space, we need to map out these areas by using the
command **bank ram** in the monitor, and then **bank cpu** after saving.

### Solution 2: Freezing

Freezing means saving the state of the machine to disk along with some
unfreezing code. This can then later be unfrozen, effectively restoring
the C64 to the state it was in when doing the freeze. On a C64, this can
be done by cartridges such as the Action Replay cartridge. However, C64
cartridges can only create imperfect freezes as some chip internals
(SID...) cannot be read. However, in emulation (i.e., VICE), snapshots
can be taken that completely contain the machine's state. This snapshot
data can then be converted into a C64/DTV .prg. This is what
[VSFReanimator](VSFReanimator "wikilink") does.

Freezing is comfortable but somewhat messy. Since the freezer cannot
determine what memory areas are actually used by the program frozen, the
snapshot taken is likely to contain leftover data, thus being larger
than necessary.

Note that files extending above $CFFF cannot be loaded with normal
floppy LOAD. See
[here](DTV_Version_2/3#Some_games_from_Spiff.27s_Repository_fail_to_run_properly._Why_is_that.3F "wikilink")
for details.

### Disassembling/Patching

You can do a disassembly of the game using
[dxa](http://www.floodgap.com/retrotech/xa/) or some other disassembler.
The nice thing about dxa is that it produces perfect code to feed back
into the [xa](http://www.floodgap.com/retrotech/xa/) assembler.

Then **patch the game**. Take care to **not move about the code too
much**, unless you want to work out a complete block list to tell the
disassembler what is code and what is data. Otherwise it tries to insert
labels in the middle of the data and disassemble it, etc. But as long as
it stays in the same memory locations, we are OK. It is also possible to
do the patch in the VICE monitor and save the PRG/snapshot with the
patch applied.

If possible, put some docs about changed key mapping in the game. If
there are already instructions, change them. Otherwise it might be
possible to put information in the high score table.

Final thing is to crop the PRG file to length if this was not done
previously, and perhaps compress it with exomizer.

Then, submit the game to [DTV Patched
Games](DTV_Patched_Games "wikilink").

FAQ
---

**How to find keyboard access routines in games?**

For keyboard access, use the watch-function and set a watch on
**$DC00-$DC01**. This allows to run the game and see where the I/O ports
of the CIA are accessed. Usually this boils down to a few places, and it
is then a matter of finding out whether this is keyboard or joystick
access, since the keyboard and joysticks are decoded by the same ports
on the CIA. In other words it is not easy to tell if a routine is
reading the joysticks or keyboard, but a guess can be done from looking
at the state of the direction register of the rows of the keyboard
matrix. If this is configured as output and a value with a single bit
low, the rest high is on the port, there is a good chance that this is
decoding a row of the keyboard.

Some games call the keyboard scanner routine in the kernel, while others
implement their own (sometimes reading just a single row of the keyboard
matrix, for instance the one containing space bar). Depending on how the
data is later decoded and used by the game there are two different
options: either implement an own keyboard scanning which returns the
scan-codes that the game expects, or simply patch the places in the game
code that finds out what to do with different events.

**How to detect DTV button presses reliably?**

Since DTV buttons use the same lines as Joystick 1, you have to take
some precautions not to confuse button presses with Joy1 movement (this
is especially important for multiplayer games). The following routine
reads DTV buttons and masks out bits that are triggered by concurrent
joystick movement (for example if Joystick 1 fire button is pressed,
detection of DTV button A is disabled).

` LDY #$FF`  
` STY $DC00`  
`LOOP:`  
` LDA $DC01`  
` CMP $DC01`  
` BNE LOOP`  
` EOR #$FF`  
` DEY`  
` STY $DC00`  
` ORA $DC01`  
` INY`  
` STY $DC00`  
` EOR #$FF`  
` AND $DC01`  
` EOR #$FF`

Still, very rarely button presses are detected by this routine even if
no DTV buttons are pressed (probably due to bouncing joystick switches).
This is why for crucial actions (quit game etc.) you should use
combinations of DTV buttons (button B+C or RFire+D - this corresponds to
Joy1 left+right/up+down).

**What about games that load from disk?**

Floppy access can be detected by *watch dd00* in VICE. However, for
games that use kernal routines (i.e., they can be run in VICE without
the “true drive emulation” option), you can also set breakpoints at
kernal routines (SETNAM $FDF9 - sets filename, OPEN $F34A - opens a
file, ACPTR $EE13 - reads a byte from IEC bus). You can then step until
that routine returns, have a look at the stack ($0100-$01FF), or use the
*backtrace* command present in recent VICEplus versions to find the
calling code.

Replacing loaders in games is quite tricky. Many games decrunch data
while loading. Since we might want to patch the data loaded (for example
if keyboard routines are in there) and since decrunching takes time but
is not needed with the DTV's 2MB RAM, typically we should get the
decrunched data. Basically the idea is to find the call to the
loader/decruncher, save a full image of the memory (*bank ram, s “file”
0 0 ffff*), do the load, save memory again, do a binary compare of the
two files (see below for a simple bindiff script), and save the
difference. Decrunched data can be put into individual files, put into a
container image that gets put into DTV upper RAM, and loaded using a
patched loading routine. For many games (see examples below), putting
level files into an image file that uses the standard DTV flashrom file
system (generated by dtvmkfs and dtvpack -0), and using a slightly
patched DTV flash load routine that loads from RAM instead of ROM (see
the loader code in [Armalyte patch
ZIP](Media:Armalyte-patching.zip "wikilink")) worked fine.

`#!/bin/sh`  
`# binary diff script`  
`cat $1 | hexdump -v -C > /tmp/bindiff.1`  
`cat $2 | hexdump -v -C > /tmp/bindiff.2`  
`diff -U 0 /tmp/bindiff.1 /tmp/bindiff.2`  
`rm /tmp/bindiff.1 /tmp/bindiff.2`

**What C64 features are not emulated (properly) in the DTV?**

See [DTV Programming\#C64 emulation issues (for the
DTV2/3)](DTV_Programming#C64_emulation_issues_(for_the_DTV2/3) "wikilink").

Keyboard/joystick breakdown
---------------------------

Table taken from [Keyboard port](Keyboard_port "wikilink") and adapted

|               |         | 56321/$dc01 | Joy 2 |
|---------------|---------|-------------|-------|
|               |         | Bit 7       | Bit 6 |
| 56320 / $dc00 | Bit 7   | STOP        | Q     |
| Bit 6         | /       | ^           | =     |
| Bit 5         | ,       | @           | :     |
| Bit 4         | N       | O           | K     |
| Bit 3         | V       | U           | H     |
| Bit 2         | X       | T           | F     |
| Bit 1         | LSHIFT  | E           | S     |
| Bit 0         | CRSR DN | F5          | F3    |
| DTV           |         |             |       |
| Joy 1         |         |             |       |

-   Lines are connected to pull-up resistors so write zeros to
    'activate' line
-   Joystick movement grounds the respective line
-   Normal (kernal) keyscan writes $DC00 and reads from $DC01 - this is
    why Joy1 movement is mistaken as keypresses
    -   Theoretically, it could write to $DC01 and read from $DC00 but
        then Joy2 would interfere - also this does not work on the DTV
-   Some facts
    -   Joy1 fire pressed: $DC01 bit 4 reads 0 regardless of $DC00
        setting
    -   Joy1 fire and M pressed: same as above plus $DC00 bit 4 reads 0
        regardless of $DC01 setting
    -   DTV Button A pressed: Any value in $DC00 with bit 0 unset leads
        to $DC01 bit 4 unset and vice versa
    -   G pressed: Any value in $DC00 with bit 3 unset leads to $DC01
        bit 2 unset (also vice versa on the C64 but not on the DTV)
    -   If Joy1 and Joy2 are used, only keys STOP Q C= / ^ = , @ : can
        get detected reliably (multiple concurrent events such as
        Joy1Fire M O Space nonwithstanding)
    -   If Joy2 is pressed up, DTV extra buttons cannot get
        distinguished from Joy1 movement (button A looks like Joy1 fire
        etc.)

Examples
========

[Walkthrough: Fixing Armalyte](Fixing_Games_for_the_DTV_Example:_Armalyte "wikilink")
-------------------------------------------------------------------------------------

[Walkthrough: Fixing Druid](Fixing_Games_for_the_DTV_Example:_Druid "wikilink")
-------------------------------------------------------------------------------

[Walkthrough: Fixing Mayhem in Monsterland](Fixing_Games_for_the_DTV_Example:_Mayhem "wikilink")
------------------------------------------------------------------------------------------------

[Notes on other games](DTV_Patched_Games "wikilink")
----------------------------------------------------

Links
=====

-   [C64 RAM map](http://sta.c64.org/cbm64mem.html)
-   [C64 ROM
    map](http://www.ludd.luth.se/~watchman/fairlight/c64/c64-rom.html)
-   [xa assembler and dxa
    disassembler](http://www.floodgap.com/retrotech/xa/)

