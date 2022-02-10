## Classic library support (`+z1013`)

* [x] Native console output
* [x] Native console input
* [x] ANSI vt100 engine (text only)
* [x] Generic console
    * [x] Redefinable font (KRT)
    * [x] UDG support (KRT)
    * [x] Paper colour
    * [x] Ink colour
    * [x] Inverse attribute
    * [x] Bold attribute (KRT)
    * [x] Underline attribute (KRT)
* [x] Lores graphics (64x64)
* [x] Hires graphics (KRT - 256x256)
* [ ] PSG sound
* [ ] One bit sound 
* [ ] Inkey driver
* [ ] Hardware joystick
* [ ] File I/O
* [ ] Interrupts
* [ ] RS232

# Quick start

    zcc +z1013 -lm -o adventure -create-app adv_a.c

--or--

    zcc +z1013 -clib=ansi -lm -o adventure -create-app adv_a.c

This command will build a file called "adventure.z80", a binary file suitable to be run on the existing emulators.


# Generic console modes

* Mode 0 = 32x32 text
* Mode 1 = 32x32, 256x256px KRT graphics


# JKCEMU emulator

Download and build from http://www.jens-mueller.org/jkcemu

To run a ".z80" program use the "Datei/Laden" menu, pick your file and click on "Starten".



# KCemu Emulator

http://kcemu.sourceforge.net/

To run:

    start kcemu -0


# Z1013 related WEB Links

[The Z1013 at hc-ddr.hucki.net](http://hc-ddr.hucki.net/wiki/doku.php/z1013)


