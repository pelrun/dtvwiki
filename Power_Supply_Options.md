---
title: Power Supply Options
permalink: Power_Supply_Options
---

To power the Hummer, you have a two basic options: going through the
original battery connections with 5 to 6 volts OR remove the on board
voltage regulation components and supply +3.3V.

If there is enough room in your project enclosure for the DTV board,
then using the battery connections is probably the preferred method.
That method will use approximatley 140mA whereas supplying +3.3V
directly will uses about 120mA.

One small problem associated with supplying power through the battery
terminal, is the fact that the ON/OFF switch cuts the ground instead of
the +V. This will stop a “true” power off condition when other
peripherals are attached to the DTV board, such as keyboard and disk
drive. In other words, current will still flow through the periphals
grounds and keep the DTV from powering down properly.

Using +BAT and -BAT
-------------------

**Advantages:** Simplier to impliment

**Disadvantages:** Uses slightly more current, larger package, power
switch on ground

**-INSERT SCHEMATIC AND PICS HERE-**

Bypassing on-board voltage regulation
-------------------------------------

**Advantages:** Lower current draw, smaller package, power switch on +V

**Disadvantes:** More complicated, requires cutting of DTV circuit board

**-INSERT SCHEMATIC AND PICS HERE-**

From JTWinters forum post on PETSCII.COM:

<http://jledger.proboards19.com/index.cgi?action=display&board=dtvhacking&thread=1170964877&page=1>

The entire “power plane” of the underside of the PCB is 3.3v. All I did
was find a nice spot where the copper cross-hatching was exposed and I
soldered a wire to that. I did the same for the ground on the top plane
(check out the images below).

I would highly recommend removing all the original voltage regulation
components though. Or you might even go to the extreme like I did and
just cut that side of the board off completely.

In my design, I needed 7.5v, 5v and 3.3v. I used a 7805 to step down to
5V. To get the 3.3V, I first tried an LDO but it didn't output enough
current. I ended up using a good old LM317 adjustable regulator. I know
this solution isn't as sexy as an LDO but it worked.

<http://www.picobay.com/picodore64/top%20plane.jpg>

<http://www.picobay.com/picodore64/bottom%20plane.jpg>
