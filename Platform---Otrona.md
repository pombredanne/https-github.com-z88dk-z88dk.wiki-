#  Otrona Attachè

The Otrona Attachè computers are CP/M compatible, so the [same base library](Platform---CPM) can be used, but extra functionalities are available.

Library extras include the graphics support (320x240).


### Command Line

    zcc  +cpm -subtype=attache -create-app program.c

A .dsk disc image will be created as a result.

### Emulator hints

Please be aware that [MAME](http://www.mamedev.org/) is not yet working correctly and instabilities may happen. 

The command line options to boot CP/M and use the created disk image will be something like:

    mame attache -flop1 oattache.td0 -flop2 a.dsk

After the CP/M boot, just enter 'b:app.com' to run the program.
