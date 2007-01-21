---
title: Repository ZIP format
permalink: Repository_ZIP_format
---

Currently this page is a playground for collaboration on finding the
next generation of the [DTV-fixed games
repository](http://symlink.dk/nostalgia/dtv/fixed/) file format, and in
general, the format of files supported by [dtvmkfs](dtvmkfs "wikilink")
and [DTVFSEdit](DTVFSEdit "wikilink").

So **DO NOT** consider this documentation. It is a suggestion to what we
**may** want to do, not what we have now.

Hopefully we will come to an agreement regarding the format, and we will
be able to implement this in both [dtvmkfs](dtvmkfs "wikilink") and
[DTVFSEdit](DTVFSEdit "wikilink"). And I will need to make some changes
to the repository `:)`

/ [Spaceman Spiff](User%3ASpiff "wikilink")

Archive format
==============

[The repository](http://symlink.dk/nostalgia/dtv/fixed/) will
exclusively use ZIP archives according to the specifications presented
here. Existing archives in the repository will be updated to conform to
this standard.

The ZIP is used as a container format, because some games and programs
use multiple files, and it is therefore nice to have this grouped
together as one entity.

The ZIP archive must contain the file `index.txt` (name must be lower
case), which lists the files to be included. The format of the index.txt
will be described in [\#The index.txt](#The_index.txt "wikilink").

Files listed in index.txt to be included in the flash file system image
must have names conforming to these specifications:

-   Allowed characters are lowercase letters (a-z), underscore, numbers
    and the character %. A period (`.`) may only be used to signify the
    extension (.prg).
-   The entry in index.txt must use the correct letter case.
-   File names end with \_dtv.prg for games that have been DTV-fixed, or
    \_hum.prg for files that have been patched to work with Hummer
    Userport Joystick.

Other files may exist in the ZIP, such as `readme.txt`, describing who
patched the game/program, etc.

The index.txt
=============

The ZIP-files must contain a file called index.txt (all lower-case).

The `index.txt` contains a list of files to be included in the flash
filesystem:

`filename,c64name,,sysaddr,PRG{,options}*`

-   Lines are terminated with CR or CR+LF.
-   The \# character starts a comment, continuing to the end of line.
-   Commas are used as separators between elements, and the file names
    cannot contain comma.
-   Empty fields (“,,”) are treated as “not specified, use default”, not
    an empty string or zero.
    -   The first element is the file name in the archive (case
        sensitive). Encoding is not relevant when only using plain ASCII
        as described above.
    -   The second argument is the C64-name, i.e. the file name name
        used in the flash file system.
        -   For backward compatibility, a C64-name starting with $ will
            be hidden from the $-file listing (and the leading “$”
            removed from the file name. Files named just “$” are not
            supported.). This is deprecated, and the archive will be
            updated to exclusively use the ,NOLIST-option
        -   Lower case letters in the C64-name are converted to upper
            case. Apart from this numbers are allowed, as are a number
            of ASCII/PETSCII characters shown in [\#Special
            characters](#Special_characters "wikilink"). Other
            characters can be specified as %XX, where XX is the
            hexadecimal value of the PETSCII-character.
    -   The third argument specifies the load-address. Because the
        repository will only support PRG-files, the load address will be
        read from the file, and must therefore be empty.
    -   The fourth (optional) argument specified the sys-address, which
        will be written to the two unused bytes in the $10000-directory
        entry. Both LSMENU, Roland's Menu and DTVSlimIntro use this as
        the SYS address when starting the game/program. A value of 0
        means that the file should not be shown in the menu. A value of
        256 means the game/program should be executed by a BASIC RUN.
        **Question: should the sys-address preferably be specified as
        256, or the real address. The value 256 is somewhat arbitrary,
        and could be replaced with a keyword, such as RUN.**
    -   Additional arguments are used for options. They are separated by
        commas. Specifying the file type overrides the detection based
        on the file name. In the repository, the file type must be PRG,
        and this must be specified as the 5th argument.
        -   PRG means this file is a PRG. The load address is read from
            the PRG-file, instead of from the third argument, which must
            be empty in this case.
        -   DTV means this file is DTV-packed. This option is for
            backward compatibility, but is not supported in the
            repository.
        -   NOLIST - Do not show this file in the $-file listing.

Special characters
------------------

The C64names field uses PETSCII URLEncoding. This means that the field
is read as ASCII and can contain letters (lower-case letters are
converted to PETSCII upper-case), numbers, and the following special
characters. Any other characters must be escaped in “%XX” syntax where
XX is the hex code if the character's PETSCII value.

Allowed characters:

**To discuss:** What about just giving the decoding algorithm...

1.  ASCII $61..$7A are converted to PETSCII $41..$5A (lowercase ASCII to
    uppercase unshifted PETSCII).
2.  ASCII $23 (\#), $2C (,), $5C give an error.
3.  ASCII $25 (%) not followed by a two-digit hex (upper- or lowercase)
    gives an error.
4.  Anything not in the ASCII $20..$5D range gives an error.
5.  Then all “%xx” occurrences are converted to the corresponding
    PETSCII value (xx in hex).
    1.  The characters %00 and %FF are invalid because they will break
        compatibility with the flash directory.
    2.  In the repository, the characters $22 (") and $2A (\*) are not
        allowed (not even if encoded as %22 and %2A), because these
        interfere with the BASIC and Kernel parsing.
    3.  In the repository, the characters & (%26) and / (%2F) must be
        encoded if used.

The following characters are not allowed in the C64-name directly and
must be encoded.

| Character   | Replacement | Reason                                              |
|-------------|-------------|-----------------------------------------------------|
| `#`         | %23         | Interpreted as comment prefix in index.txt parsing  |
| `%`         | %25         | It escapes other characters                         |
| `,` (comma) | %2C         | Interpreted as field delimiter in index.txt parsing |

Examples
========

A few examples of the suggested line-format for `index.txt`:

    lsmenu_dtv.prg,INTRO,,0,PRG,NOLIST
    hard_dtv.prg,HARD'N'HEAVY,,2077,PRG
    montezuma_dtv.prg,MONTEZUMA'S REV.,,2059,PRG
    boulderdashdtv_dtv.prg,BOULDER DASH DTV 101%25,,2061,PRG
