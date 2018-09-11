#  OSBORNE 1

The OSBORNE computers are CP/M compatible, so the [same base library](Platform---CPM) can be used, but extra functionalities are available.
Library extras include the graphics support for 104x48 low resolution graphics.


### Command Line

    zcc  +cpm -subtype=osborne1 -create-app program.c

A .dsk in Osborne1 format will be generated.

### Emulator hints

Hardware emulation for 'osborne1' [MAME](http://www.mamedev.org/) works well, see below for a possible way to transfer your programs on a disk image and successfully boot.

The generated disc can be used as `-flop2` and run with 

```
b:app
```
