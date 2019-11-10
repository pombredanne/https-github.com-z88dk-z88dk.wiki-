# Hardware Summary

* z80a @ 4MHz
* ROM: IPL (4KB) + chargen (2KB)
* RAM: 64kb RAM
* MC6845: VRAM: 4KB (text) + PCG (6KB) + GRAM (48KB)
* AY8910

_This target is receiving some attention at the moment and options/features may change_

## Classic library support

* [ ] Native console output
* [x] Native console input
* [x] ANSI vt100 engine
* [x] Generic console
    * [ ] Redefinable font 
    * [ ] UDG support
* [ ] Lores graphics (64x32)
* [ ] Hires graphics (128x64)
* [x] PSG sound
* [ ] One bit sound
* [x] Inkey driver
* [ ] Hardware joystick
* [ ] File I/O
* [ ] Interrupts
* [ ] RS232


# Quick start

    IM1 mode (org at address 0)
        zcc +x1 -create-app -lm adv_a.c

    80 columns mode (still IM1)
        zcc +x1 -create-app -pragma-define:ansicolumns=80 vtstone.c

    IM2 mode (org address = 32768 or more)
        zcc +x1 -create-app -subtype=im2 -zorg=`<address>` -lm adv_a.c


An autoboot RAW disk image (.2D extension) is created automatically by appmake.
A .D88 disk image format is obtainable with the SamDisk or XBrowser88 tools.


## Generic console

The generic console supports the following screen modes:

* Mode 0: 40x25
* Mode 1: 80x25

# Do-it-yourself a boot disk (old method)

This is an uncomfortable alternative to "appmake".

The compiler will produce a binary file.
All you need to do is put that .bin on a disk image. 
Set the 'boot' flag on the file and the "load" and "start" address if necessary.


In example, with [XBrowser88](http://www.z88dk.org/tools/x1), you'd create a 
new blank disk image (Tools -> New), add in your bin file (Home -> Add), 
and mark it as bootable by right-clicking the filename, clicking 
Properties, and checking the Boot box. You only need to do that the 
first time it is added to the disk image - if you update the file, that 
Boot flag will be preserved.