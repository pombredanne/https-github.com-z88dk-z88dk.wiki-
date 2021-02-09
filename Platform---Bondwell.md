#  Bondwell 12/14

The Bondwell computers are CP/M compatible, so the [same base library](Platform---CPM) can be used, but extra functionalities are available.

Library extras include the low resolution graphics support (160x75) and the 1bit sound functions.


### Command Line

    zcc  +cpm -subtype=bondwell -create-app program.c

A .dsk disc image (and a .COM file) will be created as a result.


### Emulator hints

[MAME](http://www.mamedev.org/) was recently updated and an issue related to the floppy disk access slowness has been solved. 

The command line options to boot CP/M and use the created disk image will be something like:

    mame64 bw12 -flop1 bondwl12.td0 -flop2 a.dsk
-or-
    mame64 bw14 -flop1 bondw14.td0 -flop2 a.dsk

After the CP/M boot, just enter 'b:a.com' to run the program.
