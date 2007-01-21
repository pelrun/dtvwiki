---
title: DTV Easter Eggs
permalink: DTV_Easter_Eggs
---

C64 DTV Built-In Secret Software User's Guide

This thing has lots of fun stuff built into it that your Average Joe
just isn't going to get to enjoy. Here are some tips to help you get the
most out of the DTV's built-in “Secret Software”.

#### Tip 01 : Speed up Intro/LOAD sequence

|        |            |
|--------|------------|
| DTV1   | Works      |
| DTV2   | Works      |
| hummer | Not tested |

-   Hold down the Left Joystick Button when the blue screen
    (*LOAD“\*”,8,1*) appears
-   The simulated (slow) typing will speed up a lot then

#### Tip 02 : Hidden menu

|        |            |
|--------|------------|
| DTV1   | Works      |
| DTV2   | Works      |
| hummer | Not tested |

To show some hidden programs:

-   Wiggle the Joystick back and forth (left to right) VERY FAST shortly
    before the blue screen appears until typing starts
-   *LOAD“$”,8* will be done
-   Programs included:
    -   BASIC Prompt (C64 BASIC) including virtual keyboard
    -   [Minima Reloaded](http://noname.c64.org/csdb/release/?id=10394)
        1.1
    -   [Splateform](http://noname.c64.org/csdb/release/?id=7183)
    -   [Dwcave](http://noname.c64.org/csdb/release/?id=7947)
    -   [Cliff Diving](http://noname.c64.org/csdb/release/?id=7940)
    -   [Tinyrinth](http://noname.c64.org/csdb/release/?id=7949)

#### Tip 03 : Hidden menu program selection

|        |            |
|--------|------------|
| DTV1   | Works      |
| DTV2   | Works      |
| hummer | Not tested |

To load and run a listed program from the C64 mode list:

-   Boot to C64 mode (Tip 02), then
-   Use the Joystick to move up to a listed program
-   Hit the Left Joystick Button
-   The chosen program will load and run.

#### Tip 04 : C64 BASIC Prompt

|        |            |
|--------|------------|
| DTV1   | Works      |
| DTV2   | Work       |
| hummer | Not tested |

To go to BASIC mode:

-   Boot to C64 mode (Tip 02), then
-   Load and run (Tip 03) the listed “BASIC PROMPT” program.
-   The familiar blank C64 blue screen will come up.

#### Tip 05 : BASIC OSD keyboard

|        |            |
|--------|------------|
| DTV1   | Works      |
| DTV2   | Works      |
| hummer | Not tested |

To use the Joystick Keyboard:

-   Go to BASIC mode (Tip 04), then
-   Hold down the Left Joystick Button.
-   The Joystick Keyboard appears.
-   Release the button and the keyboard disappears.

#### Tip 06 : BASIC OSD Keyboard usage

|        |            |
|--------|------------|
| DTV1   | Works      |
| DTV2   | Works      |
| hummer | Not tested |

To type using the Joystick Keyboard:

-   Bring up the Joystick Keyboard (Tip 05), then
-   While still holding down the Left Joystick Button,
-   Move to the key you want to type using the Joystick, then
-   Release the Left Joystick Button.
-   The key you selected will appear on the screen.
-   Repeat.

The Shift Keys are “sticky”. That is, when selected they will stay in
effect until you select them again. (Consult an online C64 reference for
an explanation of the C64's keyboard layout and functions.)

#### Tip 07 : ROM Program list

|        |            |
|--------|------------|
| DTV1   | Work       |
| DTV2   | Work       |
| hummer | Not tested |

To list the programs in memory:

-   Go to BASIC mode (Tip 03), then type (Tip 06):
-   LOAD “$”,1\[enter\]
-   LIST\[enter\]
-   A huge list of programs will scroll by.

Hint: Hold down the Left Joystick Button to bring up the Joystick
Keyboard as the list scrolls. This will temporarily halt scrolling
(though it also obscures the bottom half of the screen...)

#### Tip 08 : [Entropy Demo](http://noname.c64.org/csdb/release/?id=12080)

|        |              |
|--------|--------------|
| DTV1   | Works        |
| DTV2   | Doesn't work |
| hummer | Not tested   |

To load and run the “ENTROPY” demo:

-   Go to BASIC mode (Tip 03), then type (Tip 06):
    -   0 POKE1,55:LOAD“ENTROPY”,1\[enter\]
    -   RUN\[enter\]
-   The Entropy demo will load and run.

#### Tip 09 : Entropy demo screens

|        |              |
|--------|--------------|
| DTV1   | Not tested   |
| DTV2   | Doesn't work |
| hummer | Not tested   |

To page through different screens in the Entropy demo:

-   Load and run the Entropy demo (Tip 08), then
-   Hold down the A Button and move the Joystick to the Up position.
-   Hold for a couple of seconds and release.
-   The next screen will load and run in a few seconds.

There are 5 different screens.

#### Tip 10 : DTV Doc Viewer

|        |            |
|--------|------------|
| DTV1   | Works      |
| DTV2   | Doesn't    |
| hummer | Not tested |

To list programming info for the new graphics modes:

-   Go to BASIC mode (Tip 04), then type (Tip 06):
    -   LOAD“DOCVIEWER”,1,1\[enter\]
    -   RUN\[enter\]
-   The first page of info will display.
-   Scroll through more screens by hitting the Left Joystick Button.

#### Tip 11 : Hidden messages

|        |            |
|--------|------------|
| DTV1   | Not tested |
| DTV2   | Works      |
| hummer | Not tested |

To see the simple text Easter Egg:

-   Go to BASIC mode (Tip 04), then type (Tip 06):
    -   POKE53280,0:POKE53281,0\[enter\]
-   The screen will turn black, which makes some text (which was already
    there!) visible around the BASIC header at the top of the screen.

#### Tip 12 : Messages

|        |            |
|--------|------------|
| DTV1   | Not tested |
| DTV2   | Works      |
| hummer | Not tested |

To see the slightly less simple text Easter Egg:

-   Go to C64 mode (Tip 02), then
-   Use the Joystick to move up to the line that says 0 BLOCKS FREE and
    hit and release the Left Joystick Button.
-   You will see a name in the upper left-hand corner of the screen.
-   Hit the button again and the name changes.
-   Repeat.

#### Tip 13 : Jim & friends picture

|        |            |
|--------|------------|
| DTV1   | Works      |
| DTV2   | Works      |
| hummer | Not tested |

To see a picture of a Commodore Legend Drinking Beer:

-   Go to BASIC mode (Tip 04), then type (Tip 06):
    -   LOAD“1337”,1,1\[enter\]
    -   RUN\[enter\]

For the DTV2

-   Go to BASIC mode (Tip 04), then type (Tip 06):
    -   0 POKE1,55:LOAD“1337”,1\[enter\]
    -   RUN\[enter\]

Voila. A grainy picture of Jim Butterfield and friend.

#### Tip 14 : Jeri & friends picture

|        |            |
|--------|------------|
| DTV1   | Works      |
| DTV2   | Works      |
| hummer | Not tested |

To see a picture of the DTV Development Team:

-   Go to BASIC mode (Tip 04), then type (Tip 06):
    -   LOAD“DTVTEAM”,1,1\[enter\]
    -   RUN\[enter\]

For the DTV2

-   Go to BASIC mode (Tip 04), then type (Tip 06):
    -   0 POKE1,55:LOAD“DTVTEAM”,1\[enter\]
    -   RUN\[enter\]

Voila again. Jeri and friends.

#### Tip 15 : California Games minigames BMX and Footbag

|        |       |
|--------|-------|
| DTV1   | Works |
| DTV2   | ????  |
| hummer | ????  |

I only dusted down my DTV1 there and saw the full california games
wasn't on it. While the Frisbee and Surfing subgames are on the main
menu, the footbag (possibly because the graphics are a bit gammy) and
the BMX games aren't. However the ROMs ARE on the virtual drive, so they
can be played. I'm not sure what happened there - I think the full game
was on DTV2, and I can't find the skateboard subgame, so maybe it was
just a rush job? Anyway, if you want to play the BMX and Footbag
subgames, follow the instructions below. I'm surprised I haven't seen
this on any other DTV1 guides?

BMX

-   Go to BASIC mode (Tip 04), then type (Tip 06):
    -   0 POKE1,55:LOAD“BMX”,1\[enter\]
    -   RUN\[enter\]

Footbag

-   Go to BASIC mode (Tip 04), then type (Tip 06):
    -   0 POKE1,55:LOAD“FOOTBAG”,1\[enter\]
    -   RUN\[enter\]

