---
title: Hummer Kernal disassembly
permalink: Hummer_Kernal_disassembly
---

The Hummer (NTSC) ROM disected
==============================

More information can be found on the pages by Daniel Kahlin, see [his
page](http://www.kahlin.net/daniel/dtv/flash/hummer_kernal.txt).

Directory Format
----------------

The directory is located at $010000-$013fff.

Each entry is 32 bytes long:

  
$00-$17 Filename max 24 bytes, padded with $00

$18-$1a Location of the file in flash (*code*/*len*/*offset* data).

$1b-$1d Load address of file in RAM.

$1e-$1f Offset of *raw* file data relative to location of the file in
flash ($18-$1a).

Directory scanning stops when the first byte of the entry is $00.

Note that the directory presented when doing LOAD“$” is just a file
named “$”, not the actual directory.

File Format
-----------

The files are compressed using a simple equal-sequence compression
algorithm.

If differs from the [DTV2 flash
format](DTV2_Kernal_disassembly#File_Format "wikilink") a bit as raw
data is stored at the end of the compressed stream instead of being
interleaved with control data.

-   Every chunk of data starts with a *code* byte.
-   If *code* is $01-$7f, that number of bytes have to be read from the
    current *raw* offset.
-   If *code* is $80-$ff, then *len*=*code* & $7f, and a second byte
    (*offset*) follows
    -   this copies *len* bytes of data from the current
        destination-$0100+*offset*.
-   If *code* is $00, then we are done.

### Example

“HELLO” loaded to $0801:

`$010018  00 00 10 01 08 00 02 00`  
`         ^^^^^^^^                location of file in flash ($100000)`  
`                  ^^^^^^^^       load address ($000801)`  
`                           ^^^^^ raw data at $10000+$02=$010002`  
`$100000  05 00 08 05 0C 0C 0F`  
`         ^^^^^                   read five raw bytes, then exit`  
`               ^^^^^^^^^^^^^^    five raw bytes`
