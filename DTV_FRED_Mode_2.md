---
title: DTV FRED Mode 2
permalink: DTV_FRED_Mode_2
---

### In brief

FRED Mode 2 features :

-   One 8 bit pixel is made up of (PlaneBShifter\[1:0\],ColorRam\[3:2\],
    PlaneAShifter\[1:0\],ColorRam\[1:0\])

### Description

### Practical applications

So, how can this be useful?

### Code example

To set the FRED Mode 2 :

-   The VIC Control Register 1 ($D011) should be set to :

`ECM = 1`  
`BMM = 1`

-   The VIC Control Register 2 ($D016) should be set to :

`MCM = 1`

-   The DTV [extended VIC
    register](DTV_Programming#EXTENDED_VIC_REGISTERS "wikilink") ($D03C)
    should be set to :

`LINEAR ADDRESSING = 1`  
`HIGHCOLOR = 0`

\[TO DO : add commented example\]

### References
