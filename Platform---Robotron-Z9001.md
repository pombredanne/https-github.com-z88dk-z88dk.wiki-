# Robotron Z9001, KC85/1 and KC87


## Classic library support (`+z9001`)

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
* [x] Lores graphics (80x48)
* [x] Hires graphics (KRT - 320x192)
* [ ] PSG sound
* [ ] One bit sound 
* [ ] Inkey driver
* [x] Hardware joystick
* [ ] File I/O
* [ ] Interrupts
* [ ] RS232

# Quick start

    zcc +z9001 -lm -o adventure -create-app adv_a.c

-or-

    zcc +z9001 -clib=ansi -lm -o adventure -create-app adv_a.c


This command will build a file called "ADVENTURE.TAP", a binary file suitable to be run on the existing emulators.

## Stack location

By default, the binary is located at $1000 with stack set to $0ffe

# Generic console modes

* Mode 0 = 40x24 text
* Mode 1 = 40x24, 320x192px KRT graphics


# JKCEMU emulator

The [The Java KC-Emulator](http://www.jens-mueller.org/jkcemu/) is probably the more stable and advanced tool, at the moment.     The website includes also lots of information.

To run a ".tap" program use the "Datei/Laden" menu, pick your file and click on "Starten".



# KCemu Emulator

http://kcemu.sourceforge.net/

To run:

    start kcemu -1


# Z9001 related WEB Links

[Ulrich Zander](http://www.sax.de/~zander/index2h.html) hobby pages.   Lots of technical information and software.

[Z9001 at hc-ddr.hucki.net](http://hc-ddr.hucki.net/wiki/doku.php/z9001)

[Cassette Tape transfer hints, KC 87 and others](http://hc-ddr.hucki.net/wiki/doku.php/programme:kassetten_faq)

