---
title: DTV Flash File System
permalink: DTV_Flash_File_System
---

Standard DTV ROM file system
============================

For a description of the normal DTV ROM file system (read only, device
1, directory at $010000, files at $014000) see
<http://galaxy22.dyndns.org/dtv/common/flash-files/> and [DTV2 Kernal
disassembly](DTV2_Kernal_disassembly "wikilink").

-   [DTVFSEdit](DTVFSEdit "wikilink") (GUI) can open, edit, and write
    DTV flash file systems, handling compression transparently
-   [Spiff's dtvmkfs](dtvmkfs "wikilink") can create this type of
    filesystem (command line)
    -   [tlr's dtvpack](http://www.kahlin.net/daniel/dtv/flash.php) is
        needed to compress .prgs first
-   [Spiff's dirdump](http://symlink.dk/nostalgia/dtv/dtvmkfs/) can
    extract files from an image of this filesystem (command line)

DTVFS2
======

This is a description of a to-be implemented DTV flash filesystem that
can be modified more easily and allows implementation of SAVE on the
DTV.

Interesting links:

-   <http://sources.redhat.com/jffs2/> - JFFS2 description
-   <http://freenet-homepage.de/hardwarerobby/c64/documentation.html> -
    attaching an MMC card to the user port
-   <http://jledger.proboards19.com/index.cgi?board=dtvhacking&action=display&thread=1159386785&page=1> -
    PS1 memory card on user port

Requirements
------------

-   fault tolerance - the DTV2/3's AT47 flash supposedly only allows
    about 100 erase cycles
    -   erase only if absolutely necessary (= flash full, garbage
        collection needed)
    -   wear-levelling - make sure no block gets stressed out of
        proportion
    -   if individual blocks fail (= can't get erased properly) this
        shouldn't make the whole flash unusable
-   low overhead
    -   on-medium: 2MB of flash are not much to begin with
    -   RAM requirements should be generally low and only occur on
        medium access

Not required:

-   Truncating/changing files (delete and create is enough)
-   open/read/write/close support (would require persistent handles in
    memory)

Filesystem outline
------------------

### Data structures

-   One file occupies one or more nodes à 1kB. One block is 64 nodes.
    Only complete blocks can be erased (= set to FF).
    -   64 nodes are reserved in order to keep garbage collection
        simple.
-   Node structure
    -   File number: 16 bits (ff ff: free node, 00 00: bad/broken flash
        area, f0 00: obsoleted node, other: file number)
    -   Node sequence number: 16 bits (00 00: bad/broken flash area, 00
        01: start of file + data, other: data)
        -   start of file: null-terminated filename, null-terminated
            feature list (each feature is a stream of bytes - MSB unset
            indicates end of feature), 3b data len, data

### Algorithms

-   Read file: Search through seq==1 nodes for filename, get file
    number, walk through nodes with this file number ordered by sequence
    number.

<!-- -->

-   Write file
    -   Any block containing only obsoleted nodes? Erase block.
    -   If (number of free nodes) &gt; (nodes needed + 64), save nodes
        to random free nodes. Save done.
    -   If (number of free+obsoleted nodes) &lt; (nodes needed + 64),
        fail (not enough free space).
    -   else: grab block (G) containing many obsoleted nodes randomly OR
        (small chance) any other block.
    -   Number of non-obsoleted nodes (N) in G &lt;= free nodes: Write N
        to free nodes.
    -   else: fail (user has to delete files first - should happen only
        if flash is partially bad already)
    -   Mark all nodes as obsoleted in G, erase G, check erase
        successful; no: mark bad nodes in G.
    -   Repeat (at max n times, fail otherwise) until enough free nodes
        available.
    -   TODO: A more sensible approach dealing with bad nodes would be
        good.

<!-- -->

-   Erase file: Mark all file's nodes as obsoleted.

<!-- -->

-   Filesystem verify (in case of crash/power outage): 1. Check for
    incompletely written files, mark nodes as obsolete (crash during
    file write). 2. Check for duplicate nodes (same seq/file num).
    Continue copy, mark old node as obsolete. (crash during node copy)

This does not handle unstable erases due to power outage completely.

Implementation plan
-------------------

-   Read file
-   Erase file
-   Write file
-   Garbage collection
-   Transparent compression (possibly using
    <http://www.cs.tut.fi/~albert/Dev/puzip/>)
-   Read-only D64 support?
-   Directory support?

