---
title: DTVTrans
permalink: DTVTrans
---

About DTVTrans+
---------------

![Cable
schematics|right|thumb](cable-sch.png "fig:Cable schematics|right|thumb")
DTVTrans+ is software and hardware for transferring data from a PC to
the C64DTV via a cable connected between the PC's **printer port** and
the DTV's **joystick or user port**. The software consists of a
non-interactive program to be run on the DTV and a PC command line
program that controls transfers. The software is fully usable and has
been tested under w2k and GNU/Linux, but should work fine under
NT/w2k/XP and even w98. Current transfer rates are roughly 15Kbytes/s
dependent on the load on the PC. This is roughly 38x standard C1541
speed.

DTVTrans+ has been extended by [1570](User%3A1570 "wikilink") using code
by tlr and Spiff. It is based on DTVTrans by [Daniel
Kahlin/tlr](http://www.kahlin.net/daniel/dtv/).

-   [DTVTrans](http://www.kahlin.net/daniel/dtv/cable.php) is the
    original version by tlr.
-   **DTVTrans+**, the version available on this page, is maintained by
    [1570](User%3A1570 "wikilink").
    -   Pro: flash functionality (dtvtrans sync).
    -   Pro: typing functionality (dtvtrans type).
-   [dtv2ser+usb](http://lallafa.de/blog/dtv2ser/) is another variant by
    lallafa.
    -   Pro: connects to a USB or serial port.
    -   Pro: flash functionality.
    -   Pro: typing functionality.
    -   Pro: powerful client software with many convenience functions.
    -   Con: hardware is difficult to build
        -   There is a simpler [Arduino-based
            variant](http://lallafa.de/blog/dtv2ser/arduino/).

Features of DTVTrans+
---------------------

-   Read/write DTV RAM
-   Read/write DTV flash memory
-   Use [easter egg BASIC PROMPT](DTV_Easter_Eggs "wikilink") to type
    texts

Using the type feature, it is possible to reflash the DTV with only
[Joyport 2 mod](Joystick_port "wikilink") required (= six wires: +5V is
only required for autofire joysticks).

Components/Compatibility
------------------------

For the cable, **BAT85** Schottky-barrier diodes (hole-mount type) are
recommended. If you want smaller diodes, go for **BAT48** (Minimelf) or
**BAT54** surface-mount diodes. Other low Vf-types may work, but if you
are unsure, ask someone who has experience. 1N4148's will not work.

Some motherboards, especially those in **notebooks**, may have trouble
communicating through this cable. **USB&lt;-&gt;Parport** adapters will
**not** work either.

Verified working:

-   ASUS P4PE + BAT54, Windows (tlr)
-   ASUS A7N8X Deluxe + BAT85 (streetuff)
-   ASUS A7N8X-E + 1N4148(!), Linux (tixiv)
-   ASUS M2N32 using PCI parallel port + BAT85 (spiff)
-   IBM T20 Notebook + BAT85 (streetuff)
-   IBM T21 Notebook + BAT54, Linux (tlr)
-   Intel 865PERL + BAT85 (spiff)
-   MSI KT3V + 1N5822 (nojoopa)
-   VIA EPIA-5000 + 1N5822 (nojoopa)
-   ASRock K7S8XE+ (Winbond W83697HF super i/o) + BAT54 (1570)
-   DELL GX620 + 1N4148, Linux (abraXxl)
-   ASUS A7N8X (without E) + 1N4148, Linux (abraXxl)

You do not need to connect the reset/PotX line unless you want to use
the “dtvtrans reset” command.

Getting started
---------------

DTVTrans consists of a program on the DTV side and one on the PC side.
The DTV side just listens for requests from the PC side and has no user
interface. The DTV side of DTVTrans has to be transferred and started on
the DTV somehow.

### No floppy connected to the DTV

-   Connect cable
-   Run **dtvtrans c** on the PC to tristate the printer port (DTV
    keyboard might get garbled otherwise)
-   Run the DTVTrans bootstrap BASIC program (included in download, see
    [README.txt](http://viceplus.svn.sourceforge.net/viewvc/viceplus/trunk/tools/dtvtrans/README.txt?view=markup)).
    This can be done using two ways:
    -   Boot to BASIC, manually type in the program. A keyboard
        connector is necessary for this.
    -   Use the **dtvtrans type** feature with the [easter egg BASIC
        PROMPT](DTV_Easter_Eggs "wikilink") (see
        [README.txt](http://viceplus.svn.sourceforge.net/viewvc/viceplus/trunk/tools/dtvtrans/README.txt?view=markup)).
        Only Joy2 port required for this.
    -   Type **RUN** using the DTV joystick if the typing process went
        okay.
-   **Check** the program before running it. It disables DTV keyboard so
    fixing errors after run is difficult.
-   PC: **dtvtrans boot mlboot\_<port>.prg** - copy and execute second
    stage transfer helper program (to $cf08/53000)
-   PC: **dtvtrans boot dtvtrans\_<port>.prg** - will copy (to $0801)
    and execute DTVTRANS\_<port>.PRG in DTV RAM (installs to $018000)

You probably want to use **dtvtrans sync** then to flash a [DTV Flash
File System](DTV_Flash_File_System "wikilink") image containing
DTVTrans.

### Floppy connected to the DTV

Just copy **dtvtrans\_<port>.prg** to a disk and start it on the DTV.

Usage
-----

-   Run **dtvtrans c** on the PC to tristate the printer port (DTV
    keyboard might get garbled otherwise)
-   Start dtvtrans .prg on the DTV
-   Then, use dtvtrans on the PC side to transfer data.

`usage: dtvtrans [OPTION]... `<cmd>` [OPTION]... [`<file>`]`  
  
`Valid options:`  
`-r              use raw format for files (no load address)`  
`-l              use prg format for files (has load address)`  
`-n              do not enable safe setup timing on the DTV`  
`-b `<size>`       block size (default: 1024)`  
`-p `<addr>`       port address (default: /dev/parport0)`  
`-f              force pre 1.0 protocol`  
`-v              be verbose`  
`-s              simulate/dry run (sync command only)`  
`-d              output debugging information`  
`-h              displays this help text`  
`-V              output program version`  
  
`   Prefixing ADDRESS with an `*`r`*` means reading from DTV Flash ROM instead of its RAM.`  
  
`Valid commands:`  
`clear/clr/c     clear port (tristate)`  
`read/rd/r       read mem`  
`write/wr/w      write mem`  
`verify/vfy      verify mem`  
`go/g/sys        execute mem (go)`  
`reset/rst       reset the DTV (works only if PotX line is connected to reset pin on the DTV)`  
`sleep           sleep a number of seconds`  
`sync            sync flash memory`  
`type            type text using BASIC PROMPT easter egg (start on J)`  
`boot/bt/b       send file to boot.prg/mlboot.prg`  
`info            query server type and configuration`  
`init            BASIC init`  
`load            BASIC load`  
`save            BASIC save`  
`run             BASIC run`  
`exit            BASIC exit`  
  
`EXAMPLES`  
` dtvtrans clear`  
` dtvtrans rd 0x0400-0x0800 screen.prg`  
` dtvtrans rd 0x0400,1024 screen.prg`  
` dtvtrans rd basicprg.prg`  
` dtvtrans wr cool_demo.prg`  
` dtvtrans wr 0x020000 cool_demo.prg`  
` dtvtrans -r rd r0x000000-0x200000 complete_flash_image.bin`  
` dtvtrans g 0xfce2`  
` dtvtrans type boot.txt`  
` dtvtrans sync complete_flash_image.bin`  
` dtvtrans -s sync current_flash_image.bin new_flash_image.bin`  
` dtvtrans sync current_flash_image.bin new_flash_image.bin`

Notes
-----

-   **ERROR: got/sent block, chk=$xx** problems? Try the “-f” parameter.
-   **dtvtrans sync**
    -   ...the command variant with two files as parameters (first:
        current flash content, second: content to be flashed) flashes
        only differences between the two images. It is much faster than
        the one-parameter variant that reads the whole 2MB of flash
        first, especially if used in tandem with
        [DTVFSEdit](DTVFSEdit "wikilink").
    -   ...works only with dtvtrans running in RAM. It does not work
        with dtvtrans running from Flash (i.e., dtvtrans in
        [DTVMON](DTVMON "wikilink")). Just transfer dtvtrans\_<port>.prg
        from the DTVTrans+ download and start that as a workaround.
        Otherwise sync will bail out after transfer of the first 64k
        block (error message “NO DTVTRANS FOUND IN RAM” on the DTV).
    -   ...should be used with images that contain any crucial programs
        (menu, dtvtrans.prg, ...) first. Otherwise, in case
        transfer/flashing fails after writing the directory (part of the
        first 64k block) but before writing the actual file data (later
        blocks), files will not be accessible.
    -   ...uses image files compatible with
        [VICEplus](http://viceplus.sourceforge.net). Test images in
        VICEplus before flashing.
    -   ...cannot flash $00xxxx area. This is a safety feature.
    -   ...erases flash sectors only if needed (= bits currently set to
        0 have to be reset to 1).
    -   ...waits 20 seconds for each 64k block to let the flash helper
        finish.
-   **dtvtrans type**
    -   ...assumes that the 'cursor' in the virtual keyboard is
        positioned on 'J' on start.
    -   ...is a bit picky about timing. Background programs on the PC
        might interfere with the typing process, leading to garbled
        output.
        -   If you get garbled characters after some correct lines: (on
            the PC) stop typing (CTRL-C), *dtvtrans c*, (on the DTV)
            manually move the cursor to an empty line, enter a “J”, (on
            the PC) remove the lines already typed from boot.txt, add
            empty line at start of boot.txt, *dtvtrans type boot.txt*
-   DTVTrans/DTV side resides at $018000 and therefore blocks transfers
    of long files.
    -   To move DTVTrans+ 1.0pre1 to higher memory,
        *POKE2482,84:POKE2121,84* before first RUN or use DTVTrans in
        [DTVMON](DTVMON "wikilink") installed to Flash.
    -   DTVTrans+ 1.0pre3 can be configured via *cfg\_dtvtrans\_bank* in
        *config.asm*. Segment 84 is already default there.
-   The cable of DTVTrans and DTVTrans+ is the same. Also, the C64DTV
    client is the same for DTVTrans and DTVTrans+.

Download
--------

-   **[dtvtransplus-1.0.zip](Media:dtvtransplus-1.0.zip "wikilink")** -
    source and windows/C64DTV binaries (2008-11-24)
    -   Windows: [dtvtrans.exe](Media:dtvtrans_exe.zip "wikilink") that
        fixes *sync* not waiting 20 secs between blocks (2012-02-23)
    -   In case your DTV aborts to basic after erasing a flash block,
        quickly enter *RUN* to retry.
    -   If you encounter transfer errors, try -f and -n parameters.
    -   Compiling using a recent GCC might need [this
        patch](http://viceplus.svn.sourceforge.net/viewvc/viceplus?view=rev&revision=696).
-   Old versions
    -   [dtvtransplus-1.0pre3.zip](Media:dtvtransplus-1.0pre3.zip "wikilink") -
        source and windows/C64DTV binaries (2008-10-18: fixed flashing
        AT49BV163A)
        -   There have been reports about failing transfers with this
            version. For the time being, do *not* use the one parameter
            sync variant and *always* specify “-f”.
    -   [dtvtransplus-1.0pre1.zip](Media:dtvtransplus-1.0pre1.zip "wikilink") -
        source and windows/C64DTV binaries (2007-11-03, Win binary fixed
        2007-12-27)
        -   *type* is not reliable in this version. AT49BV163A chips
            cannot be flashed with this version.
    -   [dtvtrans-0.6.zip](Media:dtvtrans-0.6.zip "wikilink") - original
        DTVTrans without sync and type functionality (2007-01-03)

The transfer software requires giveio.sys to be installed, as it needs
direct parallel port access.  
Download: [directio\_fixed.zip](Media:directio_fixed.zip "wikilink")

-   [Browse
    repository](http://viceplus.svn.sourceforge.net/viewvc/viceplus/trunk/tools/dtvtrans/)
-   [PETSCII
    thread](http://jledger.proboards19.com/index.cgi?action=display&board=dtvhacking&thread=1875&page=1)

