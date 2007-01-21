---
title: Fixing Games for the DTV Example: Mayhem
permalink: Fixing_Games_for_the_DTV_Example:_Mayhem
---

About
-----

-   Graphics patching has been done by tlr.
-   Level loader replacement has been done by 1570.

Goals
-----

-   Fix graphics: Mayhem in Monsterland uses Variable Screen Positioning
    (VSP AKA DMA Delay) which isn't emulated perfectly.
-   Replace level loaders (load from upper RAM - package everything into
    one .prg)
-   Change cheat key configuration to DTV extra buttons

Analysis
--------

The following applies to the Warriors of the Wasteland version of
Mayhem.

### Graphics bug

-   On the DTV, there seems to be a fixed DMA delay (see 3.14.6. “DMA
    delay” in *[The MOS 6567/6569 video controller
    (VIC-II)](http://zimmers.net/cbmpics/cbm/c64/vic-ii.txt)*). Routines
    need to be fixed to manipulate registers one cycle later while not
    taking longer altogether (timing, especially on title screen, is
    pretty tight).

`> 1c43   8C 73 21   STY $2173`  
`> 2172   A9 32      LDA #$32`  
`> 2174   8D 12 D0   STA $D012`  
`> 2181   A2 12      LDX #$12`  
`> 219b   24 12      BIT $12`  
`> 219d   8E 11 D0   STX $D011`  
`> 21a3   2C 12 D0   BIT $D012`

### Disk routines

-   Level loader found by watch $dd00 and some tracing
-   Ripping decrunched levels by setting a breakpoint before load,
    saving mem, doing the load, saving mem again, diffing dumped memory.
-   Replace loader by modified version of DTV flash loader (that loads
    from RAM).

`# break before load`  
`until cc65`  
`# save memory`  
`bank ram`  
`s `“`pre-load.prg`”` 0 0 ffff`  
`# break after load`  
`until cc68`  
`# save memory again`  
`s `“`post-load.prg`”` 0 0 ffff`  
  
`ce68-cef4 core loader`  
`ccf2-ce68 decruncher`  
`cf89 <- level number`  
  
`a cc65 jsr ccf2`  
`Put loader to ccf2`

### Keyboard control

`cce3 speed extra cheat`  
`cced immortality cheat`  
`ccc4 levelskip cheat`  
  
`levelskip: RFire+D`  
`> ccc1 f3`  
`speed extra: RFire+A`  
`> cce0 e7`  
`immortality: RFire+B`  
`> ccea f6`  
  
`a 2242`  
`LDA #$FF`  
`STA $DC00`  
`LDA $DC00`  
`AND #$1F`  
`CMP #$1F`  
`BNE $2299`  
`LDA #$FE`  
`STA $DC00`  
`JSR $CCBD`  
`JMP $2299`

### Packaging everything

Save a x64 VICE snapshot. Use [VSFReanimator](VSFReanimator "wikilink")
to pack this snapshot file along with the filesystem image into one
(big) .prg.

Appendix
--------

**[Disassembled and commented code and new code
bits](Media:Mayhem-patching.zip "wikilink")** (ZIP file)
