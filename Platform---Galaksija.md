

![](images/platform/galaksija.jpg)

## Classic library support (`+gal`)

* [x] Native console output
* [x] Native console input
* [x] ANSI vt100 engine
* [x] Generic console
    * [x] Redefinable font (`-subtype=galaxyp`)
    * [x] UDG support (`-subtype=galaxyp`)
    * [ ] Paper colour
    * [ ] Ink colour
    * [x] Inverse attribute (`-subtype=galaxyp`)
    * [x] Bold attribute (`-subtype=galaxyp`)
    * [x] Underline attribute (`-subtype=galaxyp`)
* [x] Lores graphics
* [x] Hires graphics (`-subtype=galaxyp`)
* [x] PSG sound
* [x] One bit sound
* [x] Inkey driver
* [x] Hardware joystick
* [ ] File I/O
* [ ] Interrupts
* [ ] RS232

# Quick start

    zcc +gal -create-app -o adventure adv_a.c

The binary converter (appmake) will create a file named "adventure.gtp" and an audio WAV file.
"-Cz--fast" modifies the WAV file in a non standard, faster way.

The GTP file can be used directly on an emulator (i.e. MESS), or converted to WAV with the [Galaksija Tools](http://www.tablix.org/~avian/blog/articles/galaksija-tools/) by Tomaz Solc.

# Library support

## Console drivers

The Galaksija library supports native output, ANSI (vt100) and generic (VT52) output drivers.

## Galaksija Plus support

The hires screen of the Plus model is supported by both the generic console and the graphics library. The following screen modes are available:

* Mode 0: 32x16 text
* Mode 1: 256x208 monochrome graphics

Galaksija Plus support is enabled in the libraries by specifying the option `-subtype=galaxyp`

## Compact graphics library

For low memory compiles where the generic console isn't used, the original lores graphics library can be linked in using `-lgfxgaltext` 

# Emulator notes

The current version (1.9) has been tested on the "MESS" emulator; the sound library hasn't been tested.

After attaching the tape file, type the command "OLD", in the Galaksija BASIC dialect it means "LOAD".
Start the virtual tape transfer, and just type "RUN" at the prompt when LOADing is finished.


# Links

http://www.tablix.org/~avian/blog/archives/2008/12/galaksija_gets_a_c_compiler/