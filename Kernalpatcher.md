---
title: Kernalpatcher
permalink: Kernalpatcher
---

About
-----

The kernal patcher creates improved
[kernals](DTV2_Kernal_disassembly "wikilink") for the C64 DTV and the
Hummer-game. The new kernal can be tested with the patcher but to
install the kernal permanently you need to use a separate [Flash
utility](Flash_utility "wikilink").

**Note to end users: Typically only people who want to program on the
DTV might be interested in patching the kernal** (for getting the early
[DTVMON](DTVMON "wikilink") $1F8000 hook). **Another reason might be to
hardcode video timings** (to free the userport).

-   If you just want the colorfix and a different list of games, use
    [DTVSlimIntro](DTVSlimIntro "wikilink") and leave the kernal alone.
-   If you want to use JiffyDOS on the DTV, see
    [JiffyDOS](JiffyDOS "wikilink").
-   If you want to use the [TRSI
    kernal](http://noname.c64.org/csdb/release/?id=71856&show=summary),
    put the RAM version of that kernal (kernal.prg) in the games list.

Activating/installing a kernal using kernalpatcher can be done by

-   replacing the original kernal at $00e000-$00ffff in flash using the
    [Flash utility](Flash_utility "wikilink") (dangerous - errors will
    make the DTV unusable)
-   flashing the patched kernal to another address ($xxe000) and
    activating using a small helper program (“V” functionality)
-   load the patched kernal into RAM to $xxe000 and activating using a
    small helper program (“W” functionality)

The kernalpatcher has been created by [Daniel
Kahlin/tlr](http://www.kahlin.net/daniel/dtv/).

Features
--------

For features of the original DTV kernal see [DTV2 Kernal
disassembly](DTV2_Kernal_disassembly "wikilink").

-   Create patched kernal that can be flashed using the [Flash
    utility](Flash_utility "wikilink") - “C”
    -   REVERSE CTRL-KEY FUNCTION IN BOOT? N (Hummer: Y)
        -   Holding CTRL normally exits to BASIC. Selecting Y exits to
            BASIC \_unless\_ CTRL is held down.
    -   REPAIR FLASH LOAD ROUTINE? Y (Hummer: Y)
        -   Fixes the end address reported by load.
    -   PATCH TO CHECK FOR RESET HOOK IN FLASH AT $1F8000 + IMPROVE
        DIRECTORY SUPPORT, APPLY PATCH? Y (Hummer: Y)
        -   If Y, When reset $1F8004 is checked for $C4 $D4 $D6 $38 $30
            (DTV80) and jumps indirect to $1F8000 (mapped to $8000) if
            found. Holding Joystick down skips this check. (JoyA0 on the
            Hummer) Directory scanning is modified to skip entries
            beginning with $00 and terminate on entries beginning with
            $ff.(the original kernal terminates on entries beginning
            with $00)
    -   HARDCODE VIDEO TIMING? Y (Hummer: Y)
        -   The DTV normally sets the video timing from the user port
            pins. If Y, skip this check and set hardcoded values. The
            suggested defaults should be ok. (PAL for the DTV and NTSC
            for the Hummer)
    -   IMPROVED PALETTE (ZEE)? Y (Hummer: N)
        -   Improved C64 colors on PAL systems.
    -   AUTOLOAD DTVMENU INSTEAD OF INTRO? N (Hummer: N)
        -   Normally the file “INTRO” will be loaded as default when no
            $1F8000 hook or CTRL-key is detected. Saying yes here loads
            “DTVMENU” instead.
    -   BIGFILE LOAD PATCH
        -   In SVN only. This allows to LOAD files from IEC that extend
            into upper memory ($010000 and above). This breaks autostart
            programs. Handy for DTV programmers and for VICE autostart
            with big DTV files normally intended for putting into flash.
-   The kernal buffer is at $6000-$8000.
-   Save/load kernal from disk (RAW files/8192 bytes as well as PRG
    files/8194 bytes are supported)
-   Install custom kernal in RAM at $01e000-$01ffff and activate (“T”)
-   Install optional hook at $018000 to activate custom kernal in RAM
    with Left Button + Reset
-   “Softkernal” in RAM (“W”): Save RAM kernal loader to disk (copying
    patched kernal to upper RAM and activating)
-   “Softkernal” in Flash (“V”): Save flash kernal switcher to disk
    (activating a kernal flashed to $xxe000)
-   Built-in help text.
-   Includes pre-patched kernals.

Limitations
-----------

-   The ram test hook only works with the original DTV V2 (or
    compatible) kernal in flash.
-   A modified palette is not setup straight after reset. It is setup at
    the first call to $ff81/$ff5b though, so the impact is minimal. This
    can be noticed when having no dtvboot installed, going directly to
    basic using <ctrl>. Pressing Esc/Pause-Break sets up the colors.

Download
--------

[kernalpatcher-1.0.zip](Media:kernalpatcher-1.0.zip "wikilink")

[Browse SVN
repository](http://viceplus.svn.sourceforge.net/viewvc/viceplus/trunk/tools/kernalpatcher/)
