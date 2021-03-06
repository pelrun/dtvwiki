---
title: Fixing Games for the DTV Example: Armalyte
permalink: Fixing_Games_for_the_DTV_Example:_Armalyte
---

Goals
-----

-   Fix graphics: Middle section of upper/lower border is white instead
    of black
-   Replace level loaders (load from upper RAM - package everything into
    one .prg)
-   Change key configuration to DTV extra buttons
-   Add autofire function
-   Perhaps add another coop multiplayer mode (1p ship, 1p satellite)

Analysis
--------

<img src="Armalyte_idle.png" title="fig:Raster timing when idle" alt="Raster timing when idle" width="200" />
<img src="Armalyte_busy.png" title="fig:Raster timing when busy" alt="Raster timing when busy" width="200" />
<img src="Armalyte_skipburst.png" title="fig:Raster timing with skip+burst active" alt="Raster timing with skip+burst active" width="200" />
The following applies to the Remember version of Armalyte without
trainers and fastload disabled.

-   Load/run Armalyte in VICEplus. ALT-H. Save memory. Disassemble
    $9000-$CFFF,$E000-$FFFF using dxa.

### Graphics bug

-   Armalyte puts sprites into the lower border. This is done by playing
    with RSEL between scan lines $f7-$fb, effectively disabling vertical
    borders (see 3.9 “The border unit” in *[The MOS 6567/6569 video
    controller
    (VIC-II)](http://zimmers.net/cbmpics/cbm/c64/vic-ii.txt)*).
    -   The DTV doesn't emulate idle fetches (= background color in
        border if border turned off) properly in this case. Instead of
        the VIC's internal idle fetch value, it uses $D021 (background
        color) in the area of the deactivated border. Thus, we have to
        set $D021 to 0 from raster line 250 to 55 and then back to 1
        (otherwise main game graphics will look strange since all pixels
        supposed to be white are displayed black).
-   IRQ routine is at $A05F (see $FFFE). It's triggered by VIC raster
    only.
    -   The JMP destination at $A06A gets modified:
        -   $A07C handles raster line 250 (lower border, disables border
            via RSEL) and line 0 (setting up sprite handling IRQ,
            modifying $A06A)
        -   $A319 is the sprite handling routine.

To get a better idea of what is actually happening concerning raster
IRQs, a routine that changes border color whenever we are in an IRQ
handling routine can be used, see screenshots.

**Good news:** The routine handling line 0 can probably be extended to
wait up to the border and then set background color back to white - no
other IRQs are scheduled for that raster space. **Bad news:** On rare
occasions, the routine takes too long so setting background color would
happen too late, resulting in some flickering at the top. **Solution:**
Speed up the IRQ handler in line 0 by temporarily activating the DTV's
skip+burst mode. Actually, since the game seems to base its timing
completely on raster timing, keeping skip+burst enabled all the time
would probably also work. However, we don't do this to avoid potential
sublime side effects.

### Disk routines

-   -   Level loader found by watch $dd00 and some tracing
    -   Copied from $be00 to $0400 by routine at $b7f8
    -   Uses kernal i/o routines and decrunches while loading
    -   Jumped to by routine at $b3b8/$b3d7, filename set by $ffbd (name
        at $b3e9 - $b3ea is overwritten to select level)

-   Getting decrunched level data...
    -   *until b3bb*
    -   write level number (starting at 0) to $b002 using *&gt; b002 XX*
    -   *f 0700 afff 00*
    -   *f c000 cfff 00*
    -   *f e000 fff0 00*
    -   *until b3de*
    -   Take a look at memory - all levels use $0800-$8000
    -   *s “A(level)” 0 0800 8000*
-   Use dtvpack/dtvmkfs to build image of A0,A1,... files - make sure at
    $01dxxx (color ram) is dummy data
-   Replace loading routine at $be00 by a patched version of DTV flash
    load that loads from $010000 RAM (not flash)

<!-- -->

-   *&gt; b6e0 0c* (disable highscore save - found by searching for
    $ffd8 SAVE calls)
-   *&gt; b3db 0c* (disable some level loading optimization)

### Autofire

-   Fire routines found using *watch dc00* and *much* tracing
-   for single player, $ECA6 is super shot while $EC9E is normal shot
-   wrote a routine that switches normal/super show behaviour: long
    button press is autofire, bursts are super shot
    -   routine uses own timer that keeps track of timing, feeding back
        to original code 'emulated' old timing
    -   routine is put to mem freed by shorter load routine which is a
        bit tricky

### Keyboard control

-   At $AB53 - found by searching for LDA $DC01 and some tracing
-   Completely replaced by own routine that scans DTV buttons

### Packaging everything

Save a x64 VICE snapshot. Use [VSFReanimator](VSFReanimator "wikilink")
to pack this snapshot file along with the filesystem image into one
(big) .prg. This file can get compressed using DTV Exomizer (todo).

Appendix
--------

**[Disassembled and commented code and new code
bits](Media:Armalyte-patching.zip "wikilink")** (ZIP file)

### Raster coloring routine

`  .byt $80, $9f`  
`  * = $9f80`  
  
`/*`  
`Enter this in VICEplus to install hooks:`  
`> a065 4c 80 9f`  
`> a075 4c 91 9f`  
`> a080 4c 9d 9f`  
`*/`  
  
`/* drop-in for $a065 - raster IRQ handler */`  
` lda #$01`  
` sta $d019 /* IRQ handled */`  
  
` inc $9f7f`  
` lda $9f7f /* Color border */`  
` sta $d020`  
  
` jmp $a06a /* old IRQ */`  
  
`/* drop-in for $a075 - return from interrupt */`  
` lda #$00`  
` sta $d020`  
  
` lda $0e`  
` ldx $0d`  
` ldy $0c`  
` rti`  
  
`/* drop-in for $a080 - raster line 250 handler */`  
` lda #$01`  
` sta $d020`  
` sta $9f7f`  
  
` lda #$00`  
` sta $d012`  
` jmp $a085`

### Routine for fixing border

`  .byt $80, $9f`  
`  * = $9f80`  
  
`/*`  
`Enter this in VICEplus to install hooks:`  
`> a32e 4c 80 9f`  
`> a38b 4c 80 9f`  
`> a3e8 4c 80 9f`  
`> a445 4c 80 9f`  
`> a4a2 4c 80 9f`  
`> a4fb 4c 80 9f`  
  
`> a33d 4c 98 9f`  
`> a376 4c 98 9f`  
`> a39a 4c 98 9f`  
`> a3d3 4c 98 9f`  
`> a3f7 4c 98 9f`  
`> a430 4c 98 9f`  
`> a454 4c 98 9f`  
`> a48d 4c 98 9f`  
`> a4b1 4c 98 9f`  
`> a4ea 4c 98 9f`  
`> a50a 4c 98 9f`  
`> a548 4c 98 9f`  
  
`> a080 4c b1 9f`  
  
`> a319 4c d0 9f`  
`*/`  
  
`/* replacement for jmp $a06d - set next raster IRQ, return from IRQ */`  
` eor #$ff`  
` adc $d012`  
` sta $d012`  
` jsr waitForBorder /* this is new */`  
` .byt $32, $99, $a9, $00, $32, $00 /* disable skip+burst */`  
` lda $0e`  
` ldx $0d`  
` ldy $0c`  
` rti`  
  
`/* replacement for jmp $a118 - all sprites done, next raster IRQ at lower border */`  
` jsr waitForBorder`  
` .byt $32, $99, $a9, $00, $32, $00 /* disable skip+burst */`  
` jmp $a118`  
  
`waitForBorder`  
` ldy #$33`  
`loop`  
` cpy $d012`  
` bcs loop`  
`color = * + 1`  
` ldy #$ff  /* variable */`  
` sty $d021`  
` rts`  
  
`/* drop-in at $a080 - at raster $fa */`  
` lda #$00`  
` sta $d012`  
` sta $d011`  
` sta $0f`  
` lda #$fb  /* raster $fb is the right place to switch to black */`  
`waitForFB`  
` cmp $d012`  
` bne waitForFB`  
` lda $d021`  
` sta color`  
` lda #$00`  
` sta $d021`  
` jmp $a08a`  
  
`/* drop-in at $a319 - start sprite raster IRQ */`  
` .byt $32, $99, $a9, $03, $32, $00 /* enable skip+burst */`  
` ldy $12`  
` ldx $11`  
` jmp $a31d`
