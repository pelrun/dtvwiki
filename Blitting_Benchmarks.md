---
title: Blitting Benchmarks
permalink: Blitting_Benchmarks
---

Based on a
[discussion](http://jledger.proboards19.com/index.cgi?board=dtvhacking&action=display&thread=1178924643)
occurred on PETSCII DTV Hacking forums and in order to help newcomers to
jump on board and understand what the DTV can do (in addition to emulate
properly a C64), the following benchmarks program was done :

##### Goal :

-   Blit 1024 bytes of data on the screen
-   Use the 8bit chunky screen mode (320x200, 8 bits per pixel, 256
    colors table)
-   Use the following methods :

1.  Standard C64 method (Set segment mapper to map screen buffer, 1024
    stores)
2.  Standard method with DTV “burst” and “skip internal cycle” modes
    enabled
3.  DTV DMA transfer
4.  DTV Blitter operation

##### Code key points :

\[CODE KEY STEPS TO BE ADDED\]

##### Results :

This screenshot shows the program output result, the rasterbars were
used to quantify the amount of time (in HBLs) needed by each method to
blit the data on the screen :

-   Red rastertime is the fastest fillspeed possible on a normal C64,
    1024 consecutive STAs.
-   Green rastertime is the same method (1024 STA) but with the DTV in
    burst+skip cycles mode (sac \#$99, lda \#$03) and with coded aligned
    on four-byte boundaries to maximize the effect of burst+skip mode.
-   Blue is using the DMA.
-   White is using the blitter (one source, no transparency checks).

![Visual results](Blitting_benchs_results.jpg "Visual results")

##### Conclusion :

\[CONCLUSION TO BE WRITTEN\]

##### Source Code :

The source code is provided with the authorization of its author
(Shadow/Noice) and is free to use and modify.

[Source code (Kick Assembler format)](:Media:filltest.zip "wikilink")
