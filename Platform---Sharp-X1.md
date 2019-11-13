# Hardware Summary

* z80a @ 4MHz
* ROM: IPL (4KB) + chargen (2KB)
* RAM: 64kb RAM
* MC6845: VRAM: 4KB (text) + PCG (6KB) + GRAM (48KB)
* AY8910

This machine can run software natively (+x1), via CP/M (+cpm -subtype=x1) and SOS (+sos)

_This target is receiving some attention at the moment and options/features may not be in nightly builds__

## Classic library support

* [ ] Native console output
* [x] Native console input
* [x] ANSI vt100 engine
* [x] Generic console
    * [x] Redefinable font 
    * [x] UDG support
* [ ] Lores graphics 
* [x] Hires graphics (320x200 + 640x200)
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

    IM2 mode (org address = 32768 or more)
        zcc +x1 -create-app -subtype=im2 -zorg=`<address>` -lm adv_a.c


An autoboot RAW disk image (.2D extension) is created automatically by appmake.
A .D88 disk image format is obtainable with the SamDisk or XBrowser88 tools.


## Generic console

The generic console supports the following screen modes:

* Mode 0: 40x25, graphics 320x200
* Mode 1: 80x25, graphics 640x200

### Custom fonts/udgs

The PCG ram is used for custom fonts and UDGs, the PCG ram can only be loaded when in mode 0.

Loading data into the PCG is horrendously slow, so it's recommended that your application present a "Please wait" style screen whilst this is happening. The library will automatically load any compile configured `CRT_FONT` without such a screen so it's recommended that fonts are configured using the `GENCON_SET_FONT32` ioctl.


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