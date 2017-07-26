# Robotron Z9001, KC85/1 and KC87



# Quick start

    zcc +z9001 -lm -o adventure -create-app adv_a.c

-or-

    zcc +z9001 -clib=ansi -lm -o adventure -create-app adv_a.c


This command will build a file called "ADVENTURE.TAP", a binary file suitable to be run on the existing emulators.

It is possible to choose between a fast VT/ANSI color emulation and a tiny console support based on BIOS calls.


You can also change the program location with the "-zorg=" option.


The default position is at address $2000, please note that the stack is positioned just before the program location (by default the stack is at $1FFD), so changing the program position will affect the stack size.


# Build options


*  **-clib=ansi**  -  enable support for the full color ANSI VT emulation

*  **-subtype=kcc**  -  appmake variant to create a '.KCC' file rather than using the '.TAP' format

*  **-lgfx9001**  -  link the standard graphics library

*  **-lgfx9001krt**  -  link the High Resolution library for the KRT video adapter


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

