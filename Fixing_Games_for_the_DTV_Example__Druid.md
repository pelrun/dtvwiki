---
title: Fixing Games for the DTV Example: Druid
permalink: Fixing_Games_for_the_DTV_Example:_Druid
---

(back to [Fixing Games for the
DTV](Fixing_Games_for_the_DTV "wikilink"))

Druid controls
--------------

-   Singleplayer uses Joystick port 1 and seven different keys (movement
    keys nonwithstanding)
-   Two player uses both joysticks plus same keys
-   One/two player mode can be switched in-game

Goals when fixing for the DTV
-----------------------------

-   Get rid of anything requiring 'real' keyboard input (decrunchers,
    highscore entry, ...)
-   Change singleplayer so that it uses port 2 and (combinations of) DTV
    extra buttons
-   Change multiplayer to use both joysticks and (combinations of) DTV
    extra buttons
    -   ...so that we can play multiplayer with as little hardware as
        possible :-).

Taking a peek at Druid
----------------------

-   Run game in VICE, ALT-H
-   M a bit
-   Code seems to be at 8000-9fff (some at c000)
    -   *save “/tmp/druid.prg” 0 8000 cfff*
    -   *dxa -a dump /tmp/druid.prg &gt; /tmp/druid.asm*
-   Look through disassembly - data segments seem to be at 8579-858e and
    914a-9152
    -   *dxa -a dump -b 8579-858e -b 914a-9152 /tmp/druid.prg &gt;
        /tmp/druid.asm*
-   Search for dc00/dc01. Keyboard/joystick routines seem to start at
    858f.
    -   Study these routines.

Analysis
--------

-   See [\#Disassembly](#Disassembly "wikilink"). For C64DTV
    keyboard/joystick details, see [Keyboard/joystick
    breakdown](Fixing_Games_for_the_DTV#Keyboard/joystick_breakdown "wikilink").
-   Since key matrix and joystick lines overlap, detection of some
    keypresses is disabled when joysticks are moved
    -   Only STOP and C= work in every case
    -   HOME/LIRA/+/- are read only if no joystick is moved
    -   P (and Druid movement keys) are not scanned if joystick 1 moved
-   Singleplayer patch (only) would be simple - use Joy2 instead of Joy1
    for Druid control, map keys to (combinations) of DTV extra buttons
-   Multiplayer is difficult since movement of Joy1 interferes in
    detection of DTV extra buttons
    -   Using Joy2 for Druid and Joy1 for Golem would be possible but
        DTV extra button detection would have to be disabled if Joy1
        movement detected. Druid to Golem: “Hey, stop moving, I want to
        cast Chaos!” - impractial.
    -   So we have to revert to using Joy1 for Druid and Joy2 for Golem.
        Detection of DTV extra buttons still needs to be disabled when
        Joy1/Druid moves but since Druid player can coordinate this it
        should be okay (partially same as in original game anyways).
-   In this case, I just rewrote the whole keyboard/joystick routine
    (see [\#Patched code](#Patched_code "wikilink")).

<!-- -->

-   Disable highscores (read from keyboard, save to disk)
    -   Highscore read from keyboard using kernal routine. Patch $1A0E.
        Found with ALT-H and *n* stepping on highscore entry.
    -   Highscores save using kernal routine. Patch $C962. Found by
        stepping further after highscore entry.

Patch
-----

-   Patch disassembly. Compile with xa.
-   Add instructions (in high score table)

`> 1180 `“`A`` ``MIS`”  
`> 1188 `“`B`` ``KEY`”  
`> 1190 `“`C`` ``INV`”  
`> 1198 `“`D`` ``GOL`”  
`> 11A0 `“`RFIRE`”  
`> 11A8 `“`GOLCT`”  
`> 11B0 "A RF "`  
`> 11B8 `“`PAUSE`”  
`> 11C0 "D RF "`  
`> 11C8 `“`CHAOS`”

-   *load “/tmp/druid-patched.prg” 0*
-   *dump “druid-patched.vsf”*

Use VICE snapshots during patching - makes trial/error much easier.

Create .prg from *druid-patched.vsf* using
[VSFReanimator](VSFReanimator "wikilink").

Appendix
--------

### Disassembly

`               .word $8577`  
`               * = $8577`  
  
`             singleBitTable:`  
`             l8579 = * + 2`  
`               /* used somewhere else, too - don't change */`  
`               .byt $01,$02,$04,$08,$10,$20,$40,$80`  
`             rowSelector:`  
`               .byt $bf,$bf,$fd,$fb,$7f,$df,$7f,$7f`  
`             columnSelector:`  
`               .byt $04,$80,$10,$80,$10,$02,$20,$80`  
`             l858f:`  
`               /* Output of this routine: */`  
`               /* 0344: Druid movement plus bit 5 missile/P, bit 6 golem ctrl/C=, bit 7 pause/stop */`  
`               /* 0368: Golem movement (bit 0..4) */`  
`               /* 036b: bit 0 key/+, bit 1 invisibiliy/-, bit 2 golem/lira, bit 3 chaos/clr */`  
`               lda #$ff`  
`               sta $0344`  
`               sta $dc00`  
`               lda $dc01`  
`               ora #$e0`  
`               sta $03e9  /* Druid movement */`  
`               ldx #$07`  
`             l85a1:`  
`               lda rowSelector,x /* keyboard scan routine */`  
`               sta $dc00`  
`               lda columnSelector,x`  
`               and $dc01`  
`               bne l85ba`  
`             l85af:`  
`               lda singleBitTable,x /* keypress detected */`  
`               eor #$ff`  
`               and $0344`  
`               sta $0344`  
`             l85ba:`  
`               dex`  
`               bpl l85a1`  
`             l85bd:`  
`               lda $0369  /* Golem movement (zero on singleplayer - input) */`  
`               beq l85d1`  
`             l85c2:`  
`               lda $0344`  
`               ora #$0f`  
`               sta $0344`  
`               lda $dc00`  
`               and #$1f`  
`               eor #$1f`  
`             l85d1:`  
`               sta $0368  /* Golem movement (output) */`  
`               lda $03e9`  
`               eor #$ff`  
`               bne l85e0  /* Joy1 movement? Skip further keyscan. */`  
`             l85db:`  
`               lda $0368`  
`               beq l8601  /* Joy2 no movement? Scan for other keys. */`  
`             l85e0:`  
`               lda $0344  /* Put Druid movement plus P C= STOP flags into 0344 */`  
`               and #$e0`  
`               sta $0344`  
`               lda $03e9`  
`               and #$1f`  
`               ora $0344`  
`               ora #$20`  
`               sta $0344`  
`             l85f5:`  
`               lda #$ff`  
`               sta $dc00`  
`               eor $0344`  
`               sta $0344`  
`               rts`  
  
`             /* l8601: scan routine for CLR/LIRA/-/+, output to 036b lower nibble, go back to l85e0 */`

### Patched code

`               .word $8577`  
`               * = $8577`  
  
`             singleBitTable:`  
`             l8579 = * + 2`  
`               /* used somewhere else, too - don't change */`  
`               .byt $01,$02,$04,$08,$10,$20,$40,$80`  
`             rowSelector:`  
`               .byt $fe,$fe,$fe,$fe,$fe,$fe,$fe,$fe`  
`               /* in new routines we read only row 0 (DTV buttons) */`  
`             columnSelector:`  
`               .byt $01,$02,$04,$0c,$ff,$10,$08,$18`  
`               /* ButB, ButC, ButD, RFire+ButD, unused, ButA, RFire, ButA+RFire */`  
`             l858f:`  
`               /* Output of this routine: */`  
`               /* 0344: Druid movement plus bit 5 missile/P, bit 6 golem ctrl/C=, bit 7 pause/stop */`  
`               /* 0368: Golem movement (bit 0..4) */`  
`               /* 036b: bit 0 key/+, bit 1 invisibiliy/-, bit 2 golem/lira, bit 3 chaos/clr */`  
  
`               /* read joysticks */`  
`               ldx #$ff`  
`               stx $dc01`  
`               stx $dc00`  
`               lda $dc00`  
`               and #$1f`  
`               eor #$1f`  
`               sta $0344  /* Joy2 */`  
`               stx $dc00`  
`               lda $dc01`  
`               and #$1f`  
`               eor #$1f`  
`               sta $0368  /* Joy1 */`  
  
`               /* keyboard scan routine, reads to 0344 */`  
`               stx $036b`  
`               cmp #$00`  
`               bne finishkeyscan /* skip keyscan on Joy1 movement */`  
`               dex`  
`               stx $dc00   /* we only read one row on the DTV */`  
`               ldx #$07`  
`             l85a1:`  
`               lda columnSelector,x`  
`               eor #$ff`  
`               cmp $dc01`  
`               bne l85ba`  
`             l85af:`  
`               lda singleBitTable,x /* keypress detected */`  
`               eor #$ff`  
`               and $036b`  
`               sta $036b`  
`             l85ba:`  
`               dex`  
`               bpl l85a1`  
`finishkeyscan: lda $036b`  
`               eor #$ff`  
`               sta $036b`  
  
`               /* here: 0344 joy2, 0368 joy1, 036b keys */`  
`               lda $0369`  
`               bne multiplayer`  
  
`singleplayer:  /* Joy2: Druid, 0369: Golem */`  
`               ldx $0344`  
`               jmp store`  
  
`multiplayer:   /* Joy1: Druid, Joy2: Golem */`  
`               lda $0344`  
`               ldx $0368`  
`store:         and #$1f  /* Here: Accu-Golem, XReg-Druid */`  
`               sta $0368 /* Golem */`  
`               txa`  
`               and #$1f`  
`               sta $0344 /* Druid */`  
`               lda $036b`  
`               and #$e0`  
`               ora $0344`  
`               sta $0344 /* Druid + some keys */`  
`               lda $036b`  
`               and #$0f`  
`               sta $036b /* More keys */`  
`               rts`
