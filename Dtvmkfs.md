---
title: Dtvmkfs
permalink: Dtvmkfs
---

About dtvmkfs
=============

dtvmkfs is a tool for creating a DTV flash filesystem image containing
games and programs. The image can be used in
[VicePlus](DTV_Support_in_VICE "wikilink") or transferred to the DTV and
written to flash, e.g. using TLR's flash util. dtvmkfs is a command line
tool, whereas a GUI-tool called [DTVFSEdit](DTVFSEdit "wikilink") can be
used if you want to interactively work with the flash image.

When using dtvmkfs, a number of files will be imported, containing games
and programs that you want to put in the flash file system image.
dtvmkfs will also make a list of all the included files, and store this
in the image itself at address $10000. This address is just after the
kernel area, and is where the kernel will search for files. The
directory listing will be terminated with a block of $00, then a block
of $FF. The original DTV kernel stops scanning for file names when it
hits $00, whereas most patched kernels will skip over $00 and stop on
$FF. This allows a file to be deleted (by setting the filename to $00),
which can be done without erasing the flash.

Another directory will also be created by dtvmkfs, namely the $-file.
This is a file on the filesystem called $, which is loaded from BASIC on
the DTV when issuing this command:

    LOAD"$",1

This list need not contain all file names, but will make it more
“natural” to work with the flash from BASIC.

Before you [Flash the DTV Rom](Flash_the_DTV_Rom "wikilink"), it is
highly recommended that you try it in
[VicePlus](DTV_Support_in_VICE "wikilink"). For more instructions on how
to do this, see the [\#Usage examples](#Usage_examples "wikilink").

Command line options
====================

To find out what command line options are supported by your version of
dtvmkfs, use the `-h`-option:

    dtvmkfs -h

The option `-V` shows the version information for dtvmkfs.

    dtvmkfs -V

After the options come a list of files to be included. Since the options
start with `-`, anything starting with `-` will be treated as an option,
unless `--` is used, which terminates option processing and interprets
the remaining command line arguments as files to be included.

Because dtvmkfs uses getopt, command-line options that do not take an
argument can be combined:

    dtvmkfs -1lddki dtvrom.bin lsmenu_dtv.zip montezuma_dtv.zip ...

Including files
---------------

After the command line options, a number of files to be included
follows. Please note that it is not an error to not include any files.
dtvmkfs will still run without error, but the output is not particularly
useful.

-   If an argument starts with @ followed by a file name, this file will
    be read as a text-file containing a list of files to be included.

<!-- -->

-   If an argument is a single dash (`-`) a list of files are read from
    stdin. You may need to terminate argument processing with `--` to
    avoid this to be interpreted as an option. If you do not know what
    stdin is, don't worry; just pretend you did not read this paragraph,
    and don't use a file with the name `-`.

<!-- -->

-   Other arguments are parsed as files to be included. These can be
    either in the [repository ZIP
    format](repository_ZIP_format "wikilink"), as DTV-packed files, or
    as PRG-files. The file names should not contain comma (`,`), because
    this is used as a delimiter. PRG-files have their load address read
    from the file. In DTV-files this information is not present, so it
    should be specified on the command line. This is also true for the
    SYS-address for both PRG- and DTV-files (the SYS-address is used by
    LSMENU and DTVSlimIntro to determine what files to show). For more
    information regarding the options see the README-file distributed
    with dtvmkfs.

Importing a file system image
-----------------------------

By specifying the `-i <file>` option, a file system image can be
imported. Currently the parsing of the file system content is not
supported by dtvmkfs, so this option will only read the kernel area
(first 64kB). When combined with the `-k`-option, the kernel that has
been imported is written back to the flash file system image. This is
very useful for creating an image for use with
[VicePlus](DTV_Support_in_VICE "wikilink").

To test in [VicePlus](DTV_Support_in_VICE "wikilink") you will need to
have a kernel. A file system image is included with
[VicePlus](DTV_Support_in_VICE "wikilink"), in the file `dtvrom.bin` (in
the C64DTV-subdirectory).

Image output options
--------------------

When the flash file system image has been constructed in memory, it
should be saved to a file. This is done by specifying one of the
following options:

-   The option `-1` instructs dtvmkfs to write the image to a single
    file, which will be called flashfs.bin. This file starts at address
    $10000, unless the `-k`-option is given, in which case it starts at
    address 0. `-1` is the default if none of the image output options
    are specified.

<!-- -->

-   The option `-2` instructs dtvmkfs to write the image as two files.
    The first file will be called `t000000.bin` if the kernel area was
    included (by specifying `-k`), or `t010000.bin` otherwise. The
    second file will be called `t100000.bin`, but will only be created
    if this part of the flash is used. The two files can be transferred
    to the DTV with DTVtrans, and flashed.

<!-- -->

-   The option `-c` means that dtvmkfs should write the image in 128kB
    chunks. These will fit on a 1541 floppy (or .d64 image), and can be
    used for transferring to the DTV if you do not have a DTVtrans
    cable. This transfer is quite slow, so building a DTVtrans cable is
    recommended. The files will be called `c??0000.bin`. Only the needed
    files are actually created.

Verifying the operation
-----------------------

Debugging can be turned on with the `-d`-option. For each time `-d` is
specified, it increases the verbosity of the debugging information, up
to 5.

By specifying the option `-m` the main ($10000) directory listing will
be printed. This can be used to verify that the correct files have been
included.

Similarly, the option `-l` (lowercase L) prints the BASIC listing (the
contents of the $-file), which shows the files that will be visible from
BASIC.

$-file listing options
----------------------

The $-file created by dtvmkfs allows the listing of files to be shown
from BASIC. The appearance of the listing is inspired by the normal C64
listing. The first number shows the file size in 253-byte blocks. Then
comes the file name in quotes. and finally the file type PRG (which is
the only supported type in the listing created by dtvmkfs). Just before
the PRG, dtvmkfs will insert a colon (`:`). This allows the file to be
loaded by simply moving the cursor up to the line in the listing, and
typing LOAD, then pressing RETURN.

Normal C64 files have a file name length of 16 characters maximum. The
DTV can use 24 characters, and thus the listing must be adjusted for
this. By specifying the `-w <num>` option, the maximum file name length
in the listing can be changed. &lt;num&gt; is a number between 8 and 24.
If not specified it defaults to 16, which makes it look most like the
original C64 listing format.

The option `-H <txt>` (capital H) can be used to specify a header for
the $-file listing.

Usage examples
==============

In this section a few examples of using dtvmkfs will be given. Remember
that dtvmkfs is a command line tool, so if you are using Windows, open a
command prompt (Start → Run → `cmd.exe`), and run it from there. You
will of course need to change to the correct directory, where you want
the output file to be written.

No options (default behavior)
-----------------------------

The most simple example of using dtvmkfs to create a file system image
is without any options, but just importing a few files. It is
recommended that a file called INTRO is included, since the DTV kernel
will attempt to start this program after initialization. Examples of
INTRO-replacements are LSMENU and DTVSlimIntro.

    dtvmkfs lsmenu_dtv.zip montezuma_dtv.zip lastninja2_dtv.zip hardnheavy_dtv.zip

Assuming you have the needed files (from the
[repository](http://symlink.dk/nostalgia/dtv/fixed/)), this should run
without printing anything, and create a file called `flashfs.bin`, which
contains the created file system.

Default with debugging output
-----------------------------

Same as the previous examples, but this also prints a little debugging
information and the directory listing created:

    dtvmkfs -dd -l lsmenu_dtv.zip montezuma_dtv.zip lastninja2_dtv.zip hardnheavy_dtv.zip

Create image for testing in VicePlus
------------------------------------

To create a file system image for use in
[VicePlus](DTV_Support_in_VICE "wikilink"), we need to include a kernel.
The file `dtvrom.bin` included with VicePlus contains an image, which
has the original DTV kernel.

Create a simple file system image with kernel, INTRO-replacement and
games:

    dtvmkfs -1lddki dtvrom.bin lsmenu_dtv.zip montezuma_dtv.zip lastninja2_dtv.zip hardnheavy_dtv.zip

We could also use a file list:

    dtvmkfs -1lddki dtvrom.bin @filelist.txt

Where `filelist.txt` is text file with a list of the files to be
included.

After creating the image (`flashfs.bin`), we can test it in VicePlus,
either from the command line:

    x64dtv.exe -c64dtvromimage flashfs.bin

or by starting VicePlus and selecting File → DTV flash rom image →
Attach flash rom image.

Download
========

You can get the latest version of dtvmkfs from
[1](http://symlink.dk/nostalgia/dtv/dtvmkfs/). The tool is mainly
developed under Linux, and should compile on most Linux-based systems. A
version of dtvmkfs has been compiled for Windows (using MinGW), and can
also be downloaded from the site.

The source code for dtvmkfs is available if you want to use some bits
and pieces for other tools, or if you want to improve dtvmkfs. This also
allows porting to other systems. If you improve the tool or port it to a
different platform, please [contact
Spiff](http://symlink.dk/contact/contact.php), and I will try to include
your changes.
