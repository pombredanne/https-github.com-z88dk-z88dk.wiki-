VTech Laser 350/500/700.

# Hardware summary

* Z80 @3.694700 MHz
* 32k ROM
* 16k banking model in all 4 segments
* Banked graphics memory (custom ASIC)
* 16/64/128k memory available

## Classic library support

* [x] Native console output
* [ ] Native console input
* [ ] ANSI vt100 engine
* [x] Generic console
    * [x] Redefinable font 
    * [x] UDG support
* [x] Lores graphics
* [x] Hires graphics
* [ ] PSG sound
* [x] One bit sound
* [x] Inkey driver
* [x] Hardware joystick
* [ ] File I/O
* [x] Interrupts

# Compilation

    zcc +laser500 world.c -create-app -Cz--audio

Will create a file that will load on real hardware.

    zcc +laser500 world.c -create-app -Cz--audio -Cz--fast

Will create a file that will load on the Mame emulator (Mame reads/writes files with a different speed to the real hardware)

To load and run the file enter:

    cload
    run

# Emulator

In addition to Mame, [laser500emu](https://nippur72.github.io/laser500emu/) is available. To load a file in this web based emulator, drag and drop the generated ".bin" file onto the display and then type run to start.


# Features

## Generic console

The generic console is enabled by default on this port. The following screen modes are supported:

* Mode 0 - 40x24, 16 colours
* Mode 1 - 80x24, 2 colours
* Mode 2 - 40x24, 16 colours hires, 320x192

To select the colours in mode 1, you can use `textbackground()/textcolor()` to select background/foreground colours. They will only take effect the next time the screen is cleared.

Mode 2 is hires 320x192x16, in this mode the generic console supports custom fonts and 128 UDGs.

## Graphics

Graphical functions are supported in all 3 modes. The library automatically switches the implementation based on the current screen mode. When plotting the values set via `textbackground()/textcolor()` are respected.

In mode 2, the colour resolution is 1x8 (hires) pixels, for mode 0 the colour resolution is 2x2 (lores) pixels.

# Limitations

* The generated program will not work on the unexpanded (16k) VZ350
* Scanning the keyboard temporarily disables interrupts

# Maths Library

The Laser500 port supports the single precision (32 bit) Microsoft float library. This utilises ROM routines and provides a fairly compact alternative to the standard 48 bit maths libraries normally used by z88dk.

This mode is only available when using sccz80 as the compiler.

To compile:

    zcc +laser500 -fp-mode=mbf32 fp.c -lmbf32


# Reference

* 40 column screen layout: http://atariage.com/forums/topic/187667-any-info-on-video-technology-laser-500-computer/page-6 (80 column is monochrome so attribute bytes presumably become extra characters)
* Rom detection: http://atariage.com/forums/topic/187667-any-info-on-video-technology-laser-500-computer/page-8 at 0x66c2 in ROM
* Wikipedia: https://es.wikipedia.org/wiki/VTech_Laser_700
* Paging information: http://www.razzmoket.esy.es/memoire.htm