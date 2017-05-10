======= VEB Mikroelektronik (formerly Robotron) HC-900, KC85/2..KC85/5 =======


{{:platform:dstar-kc.jpg|}}

# Quick start

    zcc +kc -lndos -lm -o adventure -create-app adv_a.c


This command will build a file called "ADVENTURE.KCC", a binary file suitable to be run on the existing emulators.

You can change the program location with the "-zorg=" option.



# Build options


*  **-subtype=tap**  -  appmake variant to create a '.TAP' file rather than using the '.KCC' format

*  **#pragma output nosound**  -  (to be defined in the user C program) Save few bytes not preserving IX on tape and sound interrupts


# JKCEMU emulator

The [The Java KC-Emulator](http://www.jens-mueller.org/jkcemu/) is probably the more stable and advanced tool, at the moment.     The website includes also lots of information.

To run a ".tap" program use the "Datei/Laden" menu, pick your file and click on "Starten".



# KCemu Emulator

http://kcemu.sourceforge.net/

To run:

    start kcemu -2   (or -3..-5)


# WEB Links


[KC-Club Homepage](https///www.iee.et.tu-dresden.de/~kc-club/)

[Ulrich Zander](http://www.sax.de/~zander/index2h.html) hobby pages.   Lots of technical information and software.


