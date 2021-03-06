---
title: DTV FRED Mode
permalink: DTV_FRED_Mode
---

### In brief

FRED Mode, as named after Six's turtle testing program, is a C64
[FLI](http://www.studiostyle.sk/dmagic/gallery/gfxmodes.htm)-like mode.
It features :

-   160x200 resolution
-   No CPU overhead compared to classic C64
    [FLI](http://www.studiostyle.sk/dmagic/gallery/gfxmodes.htm) mode
-   Cellular mode (4x8)
-   16 colors per cell
-   One 8 bit pixel is made up of (ColorRam\[3:0\],
    PlaneBShifter\[1:0\], PlaneAShifter\[1:0\])

### Description

FRED mode is really more of a “retro” c64-ish mode and a kind of like a
CPU-less [FLI](http://www.studiostyle.sk/dmagic/gallery/gfxmodes.htm)
mode with a re-definable palette.

It gives the programmer a normal MCBM style cells.

Each 4x8 cell can contain any of 16 colors, the downside being that
those 16 colors have to have the same high nibble, which is determined
by the [ColorRam](DTV_Color_Matrix "wikilink") for that cell.

Since this is based on a standard c64 multicolor mode, each “pixel” is
two normal pixels wide, giving you a resolution of 160 pixels across. So
in 160x200, you'll have 1000 cells of 4x8, the size of the
[ColorRam](DTV_Color_Matrix "wikilink").

Thus, the core will fetch two bits from plane A, two bits from plane B,
and 4 bits from color ram to generate each pixel. This allows 16 colors
per “cell” where :

“`C`”` is `[`ColorRam`](DTV_Color_Matrix "wikilink")` for this cell`  
“`B`”` is plane B`  
“`A`”` is plane A`  
`A pixel color value will be %CCCCBBAA`

So for example :

`if %CCCC = $0 then the pixels for this cell will come from the   0-15  entries in the palette`  
`           $1                                                   16-31`  
`           $2                                                   32-47`  
`           ..                                                    ...`  
`           $F                                                  240-255`  
  
`so if :   ColorRam is $4 (=> colors will be taken between palette entries $40 and $4F (64 and 79))`  
`          byte in Plane B is %10 10 11 00`  
`          byte in Plane A is %00 01 10 10`  
  
`                              %CCCC BB AA`  
`then the pixel colors will be %0100 10 00 = $48, `  
`                              %0100 10 01 = $49, `  
`                              %0100 11 10 = $4e, `  
`                              %0100 00 10 = $42.`

### Practical applications

So, how can this be useful?

-   Say you define your palette colors $0-$f to match the normal C64
    palette. Then you could then display a standard FLI picture without
    using any CPU time, simply by converting it to a FRED-friendly
    format.
-   Need color effect ? Just change the color matrix of the desired
    cell(s) to point to another palette entries and you'll have some
    color shifting effects on your picture

### Code example

To set the FRED Mode :

-   The VIC Control Register 1 ($D011) should be set to :

`ECM = 1`  
`BMM = 1`

-   The VIC Control Register 2 ($D016) should be set to :

`MCM = 1`

-   The DTV [extended VIC
    register](DTV_Programming#EXTENDED_VIC_REGISTERS "wikilink") ($D03C)
    should be set to :

`LINEAR ADDRESSING = 1`  
`HIGHCOLOR = 1`

\[TO DO : add example from Six\]

### References

-   Read this [PETSCII
    thread](http://jledger.proboards19.com/index.cgi?action=display&board=dtvhacking&thread=1178924898&page=1)
    where Six describes the FRED Mode.

