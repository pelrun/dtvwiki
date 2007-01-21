---
title: DTV Specific Programming
permalink: DTV_Specific_Programming
---

The DTV includes several features which extend the capabilities of the
original Commodore C64.

DTV Hardware features
---------------------

-   see [front page](C64_DTV_Hacking_Wiki#DTV_Versions "wikilink") or
    [Wikipedia](http://en.wikipedia.org/wiki/C64DTV) article

Articles on DTV Features
------------------------

-   [Blitting Benchmarks](Blitting_Benchmarks "wikilink") between the
    DMA, the Blitter, the burst mode and classic C64 blitting methods
-   TODO: Playing 8 bits audio sample using DTV SID additional registers
-   TODO: Implementing stable symmetric overscan
-   TODO: Display a 16 colors picture in FRED mode

Programming Documentation
-------------------------

### For DTV1

-   [DTV Registers Full](Media:Dtv-registers-full.txt "wikilink") - the
    definitive reference by Jeri Ellsworth herself.

### For DTV2/DTV3

-   **[DTV2/3 Programming guide](DTV_Programming "wikilink")** - the
    definitive reference, originally written by Jeri Ellsworth herself.
    -   [DTV FRED Mode](DTV_FRED_Mode "wikilink") description
    -   [DTV FRED Mode 2](DTV_FRED_Mode_2 "wikilink") description
    -   [DTV2 Kernal disassembly](DTV2_Kernal_disassembly "wikilink") -
        including explanations of the DTV2/3 flash file system and user
        port bits
    -   [DTV Flash File System](DTV_Flash_File_System "wikilink") links
        and ideas
-   [DTV2/3 Reference
    Schematic](Media:DTV-Ver2-Schematic.pdf "wikilink") (PDF)
-   [Segment Mapper Tutorial, revC](Media:segmapper_1c.pdf "wikilink")
    (PDF) Explores techniques for accessing extended RAM & ROM. (by
    gmoon)

### General C64 Docs

-   **[All About Your
    64](http://unusedino.de/ec64/technical/aay/c64/)** - opcodes, memory
    maps, chip specifications, commented ROM listings, nicely
    interlinked.
-   *[The MOS 6567/6569 video controller (VIC-II) and its application in
    the Commodore 64](http://zimmers.net/cbmpics/cbm/c64/vic-ii.txt)* by
    Christian Bauer
-   Docs (mirrored) by Fairlight
    -   [Commodore 64 input/output
        Assignments](http://www.ludd.luth.se/~watchman/fairlight/c64/c64-io.html)
    -   [Commodore 64 memory
        map](http://www.ludd.luth.se/~watchman/fairlight/c64/c64-memo.html)
    -   [Commodore 64 Ram
        addresses](http://www.ludd.luth.se/~watchman/fairlight/c64/c64-ram.html)
    -   [Commodore 64 Rom
        addresses](http://www.ludd.luth.se/~watchman/fairlight/c64/c64-rom.html)

Programming Tools
-----------------

### 6510 Assemblers

-   [Turbo Assembler](http://turbo.style64.org) with native support of
    DTV opcodes
-   [Turbo Macro Pro](http://turbo.style64.org/tmp.php) - Assembler for
    6510/DTV originating from the Omikron Turbo Assembler series
-   [DTVMON](DTVMON "wikilink") - DTV specific machine language monitor
    originating in HesMon and CCSMON

### 6510 Cross-Assemblers

-   [Kick Assembler](http://www.theweb.dk/KickAssembler/Main.php) - a
    novel cross-assembler for 6502/6510/DTV. Java. No source available.
-   [xa and dxa](http://www.floodgap.com/retrotech/xa/) - xa assembler
    and dxa disassembler written in portable C. GPL. DTV support
    pending.
-   [Dasm](http://www.atari2600.org/DASM/) - a 6502 cross-assembler
    written in portable C. Source included. No DTV support. Last update
    in 2004.
-   Acme, TAss, CA65, C64Asm
-   [6502 Cross-Development Languages and Tools
    List](http://www.npsnet.com/danf/cbm/cross-development.html) - BIG
    list of cross-dev tools, compiled by Daniel Fandrich
-   [World of Fairlight](http://www.fairlight.to/tools/pc.html) -
    Fairlight's Tools, everything plus the kitchen sink

### IDE (PC)

-   [Relaunch64](http://www.popelganda.de)
-   Any good text editor with syntax highlighting and Run command line
    capabilities (to call the cross-assembler)

### Other Programming Languages

-   [cc65](http://www.cc65.org/) is a complete cross development package
    for 65(C)02 systems, including a powerful macro assembler, **a C
    compiler**, linker, librarian and several other tools. No specific
    DTV support as of June 2007.
-   [DTVBASIC](http://www.thegang.nu/dtv_basic.php) - A BASIC extension
    for the DTV, by Grokk. Includes RAMdisk and DMA support.

### C64DTV Emulators

-   [VICEplus](http://viceplus.sourceforge.net/) features C64DTV
    emulation.

### Other tools

-   [VSFReanimator](VSFReanimator "wikilink") converts a VICE snapshot
    (.vsf) to a .prg file that can be started on the DTV.

65xx Documentation
------------------

-   [6502.org](http://6502.org/) - a resource for people interested in
    building hardware or writing software for the 6502 microprocessor
    and its relatives

