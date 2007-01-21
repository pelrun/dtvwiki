---
title: GEOS for the DTV
permalink: GEOS_for_the_DTV
---

GEOS on the C64DTV/Hummer
=========================

DTV/Hummer compatible disk images
---------------------------------

Go [here](http://dtvforge.ath.cx/geosdtv/) for disk images. These can be
run in [VICEplus](http://viceplus.sourceforge.net/).

Todo
----

-   Shorten REU/DMA routine (overwrites \_ToBasic/LoKernal1 routines
    currently)
-   Extend vsfReanimator to allow x64dtv snapshots as input
-   Create driver for [PS/2 mouse on user
    port](http://galaxy22.dyndns.org/dtv/common/ps2mouse/)

Patching
--------

This info is for coders only. Users see D64 download above.

-   [Download GEOS64](http://cbmfiles.com/geos/index.html).
-   Get everything running in VICE x64.
-   Patch CONFIGURE 2.1.
-   Load GEOS in x64 with 2MB REU.
-   Create ramdisk and copy everything interesting there.
-   Go to monitor. Patch REU routines to use DTV DMA.
-   *dump “geos.vsf”*
-   Extract first 1856k of REU memory from vsf using a hexeditor
-   Prepend 64k zero bytes to REU memory (will go to $01xxxx on the DTV
    that is also used for colormem)
-   *[vsfReanimator](VSFReanimator "wikilink") geos.vsf 0 0 reumem.bin*

More information
----------------

-   <http://jledger.proboards19.com/index.cgi?board=dtvhacking&action=display&thread=1169601506>
-   <http://www.forum64.de/wbb2/thread.php?threadid=16422>
-   <http://members.elysium.pl/ytm/html/geos.html> - GEOS KERNAL source
    (for REU see lokernal.tas)
-   <http://www.zimmers.net/geos/docs/inputdrv.txt>

Code
----

`/*`  
`CONFIGURE 2.1 -  detect DTV RAM as 1856k REU`  
`a 0fb3`  
` jsr $c25c`  
` lda #$1d`  
` sta $88c3`  
` lda #$20`  
` sta $2102`  
` jmp $c25f`  
`*/`  
  
`/* Replacement REU code, XA format */`  
  
`   .word $9eaa`  
`   * = $9eaa`  
  
`l9eaa  ldy #$93   /* Verify */`  
`   bne exit`  
`l9eae  ldy #$90   /* C64 => REU */`  
`   bne transfer`  
`l9eb2  ldy #$92   /* Swap */`  
`   bne transfer`  
`l9eb6  ldy #$91   /* REU => C64 */`  
  
`transfer:`  
`   ldx #$0d    /* error flag */`  
`   lda $08`  
`   cmp $88c3   /* max segment */`  
`   bcs exit`  
`   ldx $01     /* save $01 */`  
`   lda #$35`  
`   sta $01`  
` `  
`   lda #$01    /* enable ext regs */`  
`   sta $d03f`  
  
`   lda #$00`  
`   sta $d307`  
`   sta $d309`  
`   sta $d31d`  
`   sta $d31e`  
`   lda #$01`  
`   sta $d306`  
`   sta $d308`  
  
`   lda $06`  
`   sta $d30a  /* Transfer len lo */`  
`   lda $07`  
`   sta $d30b  /* Transfer len hi */`  
  
`   cpy #$90`  
`   bne t1`  
`tc2r:  jsr c64toreu`  
`   lda #$0d`  
`   bne dodma`  
`t1:    cpy #$91`  
`   bne t2`  
`tr2c:  jsr reutoc64`  
`   lda #$0d`  
`   bne dodma`  
`t2:    jsr c64toreu`  
`   lda #$0f`  
`dodma: sta $d31f`  
`wait:  lda $d31f`  
`   bne wait`  
  
`   stx $01`  
`   ldx #$00`  
`exit   lda #$00     /* disable ext regs */`  
`   sta $d03f`  
`   rts`  
  
`c64toreu:`  
`   /* source */`  
`   lda $02`  
`   sta $d300`  
`   lda $03`  
`   sta $d301`  
`   lda #$40    /* RAM */`  
`   sta $d302`  
  
`   /* dest */`  
`   lda $04`  
`   sta $d303`  
`   lda $05`  
`   sta $d304`  
`   lda $08`  
`   clc`  
`   adc #$42    /* skip first two 64k segments, RAM */`  
`   sta $d305`  
`   rts`  
  
`reutoc64:`  
`   /* source */`  
`   lda $04`  
`   sta $d300`  
`   lda $05`  
`   sta $d301`  
`   lda $08`  
`   clc`  
`   adc #$42    /* skip first two 64k segments, RAM */`  
`   sta $d302`  
  
`   /* dest */`  
`   lda $02`  
`   sta $d303`  
`   lda $03`  
`   sta $d304`  
`   lda #$40    /* RAM */`  
`   sta $d305`  
`   rts`  
  
`   brk`
