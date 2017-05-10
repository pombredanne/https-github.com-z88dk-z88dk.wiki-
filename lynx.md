======= Camputers Lynx =======

{{:platform:lynx.gif|}}

(About 30000 units were sold).

# Quick start

    zcc +lynx -lm -create-app adv_a.c

This will create "a.tap", ready for the use on an emulator.


# "PALE" emulator method

    zcc +lynx -lm -o adventure.z80 adv_a.c

This will create a binary block called "adventure.z80".

Please use the "PALE" Pete's Lynx Emulator and get to the options window (right-click).

[Archived copy of the no longer existing PALE website](http://web.archive.org/web/20120119133246/http://heraclion.users.btopenworld.com/palelynx.htm)


Click on the "Z80-TAP" button to get in the BINARY converter tool, set both the "start" and "execute" fields to 7000 (good for a 96K Lynx computer) and click on "convert" to get the .TAP file.

The resulting .TAP file will be a "binary mode" file (to be run with MLOAD "").



