---
title: DTVMON
permalink: DTVMON
---

About
=====

DTVBOOT/DTVMON is a combination of programs intended for DTV
programmers, written by Daniel Kahlin. Typically, DTVBOOT is flashed to
$1F8000 and activated by the $1F8000 hook in a kernal patched by
[Kernalpatcher](Kernalpatcher "wikilink"). DTVBOOT can do some simple
things such as zeroing the DTV RAM, changing video out settings,
activating an alternate kernal (typically the [TRSI
kernal](http://jledger.proboards.com/index.cgi?board=dtvhacking&action=display&thread=2721)),
and launching DTVMON. DTVMON is a machine language monitor adapted for
the DTV.

Both programs can also be launched as a RAM version and without a
patched kernal (activated using the $018000 RAM reset+left fire hook
present in the [standard DTV
kernal](DTV2_Kernal_disassembly "wikilink")). The drawback of this is
that some RAM areas are occupied by DTVBOOT/DTVMON and not available for
DTV programming.

Programmers who want to program directly on the DTV will want to put
both programs and a patched kernal in the DTV flash. End users typically
do not need DTVBOOT/DTVMON at all or can use the RAM version (that can
be loaded and started using [DTVSlimIntro](DTVSlimIntro "wikilink")).

With VICE emulating the C64DTV, programmers might consider using the
much more powerful VICE monitor for DTV programming.

Weblinks
========

-   [DTVMON/DTVBOOT](http://www.kahlin.net/daniel/dtv/dtvmon.php)
    project page

