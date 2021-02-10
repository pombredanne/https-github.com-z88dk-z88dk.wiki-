#  Bondwell Model 2

The Bondwell computers are CP/M compatible, so the [same base library](Platform---CPM) can be used, but extra functionalities are available.

Library extras include the high resolution graphics support (640x200).


### Command Line

    zcc  +cpm -subtype=bondwell2 -create-app program.c

A .dsk disc image (and a .COM file) will be created as a result.


### Emulator hints

[MAME](http://www.mamedev.org/) was recently updated and an issue related to the floppy disk access slowness has been solved. 

The command line options to boot CP/M and use the created disk image will be something like:

    mame64 bw2 -flop1 bondwl02.td0

After the CP/M boot, change the virtual floppy disk image (choose A.DSK).

Alternatively, the Dunfield's tools can be used (TD2IMD, TD02IMD and IMDU) to convert the CP/M system disk image into a RAW unencoded file, suitable to be altered with cpmtools.
The file A.COM is built together with the packaged A.DSK
