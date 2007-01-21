---
title: DTVFSEdit
permalink: DTVFSEdit
---

News
====

-   2008-10-25: Several minor changes for the upcoming revised [Spiff
    ZIP](Repository_ZIP_format "wikilink") format. ZIPs written by
    DTVFSEdit are compliant now.
-   2008-10-18: Write uncompressed PRGs to Spiff ZIP. Use PETSCII-aware
    encoding in Spiff ZIP index.txt. Open images specified on command
    line.
-   2008-08-30: Several bugs in Spiff ZIP handling have been fixed.

For a complete changelog, see
[here](http://viceplus.svn.sourceforge.net/viewvc/viceplus/trunk/tools/dtvfsedit/?view=log).

About DTVFSEdit
===============

<img src="dtvfsedit_screenshot.png" title="DTVFSEdit (GTK style)" alt="DTVFSEdit (GTK style)" width="300" />

DTVFSEdit is an interactive GUI C64DTV flash file system editor written
in Java 1.5.

DTVFSEdit supports editing flash images in a fashion that allows
updating the DTV flash without erasing flash blocks. This speeds up the
flash process and especially does not impose wear on the flash chip (the
rather special AT47 flash chip used in the DTV is said to last about a
hundred erase cycles only).

**[DOWNLOAD](https://viceplus.svn.sourceforge.net/svnroot/viceplus/trunk/tools/dtvfsedit/)**
- get dtvfsedit.jar. A [Java Runtime Environment
(JRE)](http://java.sun.com/javase/downloads/) 5 or higher is needed to
run the editor.

DTVFSEdit is best used in tandem with [DTVTrans+](DTVTrans "wikilink")
(especially its *sync* functionality) and
[VICEplus](http://viceplus.sourceforge.net/) (for testing images).

Use [DTVSlimIntro](DTVSlimIntro "wikilink") as INTRO replacement. The
original INTRO/DTVMENU won't do much good with a reflashed DTV: You
neither want the lengthy intro sequence nor the original DTVMENU with
its hardcoded list of games.

Functionality
=============

-   open/save/create **DTV flash images** and **Spiff ZIPs** (using the
    *File* menu)
    -   DTV flash rom images are exactly 2097152 bytes in size
    -   Spiff ZIPs can be fetched from [Spiff's
        repository](http://symlink.dk/nostalgia/dtv/fixed/)
-   import (multiple) PRGs/Spiff ZIPs (using *Import* button)
    -   Use PRG import only if you know what you are doing. End users
        should stick to Spiff ZIPs.
-   export entry as PRG file (using *Export* button)
-   drag and drop between opened containers
-   basic/kernal status is displayed - basic/kernal can be copied from
    other images using drag and drop
-   changing name and start/load address for entries (doubleclick)
-   display free image mem
-   reserving high memory ([DTVMON](DTVMON "wikilink") etc. - simply
    every non-FF byte is marked as used)
-   supports uncompressed PRGs in Spiff ZIPs (add “,PRG” parameter to
    index.txt - load addr is fetched from PRG then)

Possible future improvements
----------------------------

-   direct download from [Spiff's
    repository](http://symlink.dk/nostalgia/dtv/fixed/)
-   direct control of [DTVTrans](DTVTrans "wikilink")
-   “$” file handling
-   speed improvements (not decompressing/recompressing files etc.)
-   use progress bars
-   many other UI improvements

Usage Examples
==============

Building a completely new image
-------------------------------

1.  Read flash image from DTV: *dtvtrans -r rd r0x000000-0x200000
    flash.image*
2.  Start DTVFSEdit: *java -jar dtvfsedit.jar* (or doubleclick on
    dtvfsedit.jar)
3.  Open flash image created in step 1: File/Open “flash.image”
4.  Create new empty flash image: File/New DTV Flash Image
5.  Copy basic/kernal to new flash image: Drag a file from flash.image
    to the lower info bar of the new image
6.  Add any Spiff ZIPs or PRGs using Import... to the new image
7.  Save new flash image: Save Image...
8.  Test image: *x64dtv -c64dtvromimage new.image*
9.  Flash image on DTV: *dtvtrans sync flash.image new.image*

Changing an existing image
--------------------------

1.  Read flash image from DTV: *dtvtrans -r rd r0x000000-0x200000
    flash.image*
2.  Start DTVFSEdit: *java -jar dtvfsedit.jar* (or doubleclick on
    dtvfsedit.jar)
3.  Open flash image created in step 1: File/Open “flash.image”
4.  For files you want to remove from DTVSlimIntro, set start address
    to 0. If you want to replace a file, shorten its name (empty string
    works only with patched kernal and is not recommended), set start
    address to 0, and add the replacement file using drag/drop
5.  Add new files as you like using Import or drag/drop from other
    images
6.  Save new image: Save Image...
7.  Test image: *x64dtv -c64dtvromimage new.image*
8.  Flash image on DTV: *dtvtrans sync flash.image new.image*

Questions and Answers
=====================

What do I have to enter in the load and start address fields?
-------------------------------------------------------------

If you imported Spiff ZIPs, these fields are already correctly set and
should not be changed. Typically, the load address is $0801 (BASIC
start), and start address $0100 (this is the SYS command executed to
start the program - $0100 is an alias for “RUN” in DTVSlimIntro and not
actually a SYS). Set start address to 0 to hide the program from the
menu.

How to delete files from the flash image?
-----------------------------------------

This is not supported directly. Create a new flash image and drag/drop
all the files to it apart from the file you want to delete. In normal
circumstances (you only want to delete a file and add a new one) you
should merely make the old file invisible (see second usage example).

“No basic or kernal present”?
-----------------------------

Open an image file that includes basic and kernal (e.g., read from the
DTV). Then drag/drop something from that image to the info bar. $00xxxx
(including basic and kernal) will be copied from the old to the new
image.

Without basic and kernal the image will not run in VICEplus.

How safe is this?
-----------------

Well, as long as you do not flash your DTV using a program that flashes
$00xxxx (the basic/kernal area), nothing much can go wrong - you can
always go to BASIC by holding CTRL during reset. Still, use
VICEplus/x64dtv to check the images generated by DTVFSEdit.

Something didn't work.
----------------------

Check the console. Some error messages might show up in the console
only.

LOAD“$”,1 from BASIC says FILE NOT FOUND...
-------------------------------------------

There's no $ handling in DTVFSEdit. Kernal LOAD doesn't really handle
building the flash directory; it merely reads a “$” file from the flash.
Generating and updating this file is quite a hassle though if you want
to support non-erase updates (as DTVFSEdit does). As the “$” file isn't
necessary really with [DTVSlimIntro](DTVSlimIntro "wikilink") (that
actually builds the directory by looking at flash content), support for
it was just dropped.

I don't have a DTVTrans cable. How do I reflash the DTV?
--------------------------------------------------------

(Let someone) build the cable. It's cheap and very handy. Otherwise you
can split the flash image (using *split* or another similar utility) and
flash it in several parts.
