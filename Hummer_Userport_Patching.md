---
title: Hummer Userport Patching
permalink: Hummer_Userport_Patching
---

Basically for games to be played with a joystick on the Hummer they need
to be patched to read from the userport rather than the joystick port.
The userport address is hex memory location $DD01. Joystick port 1 & 2
are memory locations $DC01 & $DC00. In this case it's important to note
the reverse in order versus port number so $DC00=port2 and $DC01=port1.
