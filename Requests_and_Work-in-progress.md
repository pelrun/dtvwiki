---
title: Requests and Work-in-progress
permalink: Requests_and_Work-in-progress
---

This page contains lists of games that someone are working on
DTV-fixing, as well as a list of requests. If you start fixing a game,
please enter it here, to avoid other people duplicating your work. If
you end up giving up, please remove your name from the list (and you
will be bugged accordingly on the forum ;-)

Also, requests are welcome.

Check [the repository](http://symlink.dk/nostalgia/dtv/fixed/) for
already fixed games.

Work-In-Progress
----------------

| Game    | Worked on by | Comment |
|---------|--------------|---------|
| Katakis | tixiv        |         |

Requests
--------

| Game                         | Req. by              | Comment                                           |
|------------------------------|----------------------|---------------------------------------------------|
| Action Biker                 | DarraghC.            |                                                   |
| Airborne Ranger              | DarraghC.            |                                                   |
| Beach Head (II)              |                      | Level loader. Both use TOD timer and hang.        |
| Chuckie Egg II               | matthewpaulwiltshire |                                                   |
| Delta                        |                      | Minor graphics bug in border due to idle fetch.   |
| G.I. Joe                     |                      | Level loader                                      |
| Jetset Willy                 | tribune              |                                                   |
| Krakout Prof II              | Spiff                |                                                   |
| Mini Golf (Capcom)           | Flexman              | swaps joystick port, cannot pass name entry (\*3) |
| O'Riley's Mine               |                      |                                                   |
| Oils Well                    |                      |                                                   |
| Pang                         |                      | Note: Cartridge version uses RAM at $8000, too.   |
| Panther                      | tribune              |                                                   |
| Park Patrol                  |                      |                                                   |
| Raid over Moscow             |                      | Uses TOD timer                                    |
| Rainbow Islands              |                      | Level loader, some flickering (?)                 |
| Rodland                      |                      | Loader, crashes ingame (also in x64dtv)           |
| Space Crusade                | nojoopa              | Level loader, too slow (\*2)                      |
| Speedball (full version, 2P) | Shazz                |                                                   |
| Spelunker                    | tribune              |                                                   |
| Spy vs Spy                   | tribune              |                                                   |
| Turrican II                  |                      | Minor sprite glitches in-game, level loader       |
| World Games                  | Flexman              | multipart game                                    |
| Wizball                      | ik                   | (\*1)                                             |

(\*1) There are two versions of Wizball out there. The one already
patched for DTV shows both the icons and the color pots in the lower
border, the one I'd like to see patched moves the icons to the top of
the screen so everything is visible all the time. The Remeber-Version is
one of those.

(\*2) The game is a bit slow on a C64. Enabling skip+burst makes it run
at a comfortable speed, but adds some gfx glitches.

(\*3) Multiplayer and multipart game, but can load from other devices
than 8. So only thing to be done for a fix is that the same joystick
port will be used for all players, and that you can pass the player
selection and name entry without a keyboard.
