# Hardware

* Sharp LR35902 at 4.19 MHz
* 8k VRAM
* 8k General purpose RAM
* 32k cartridge + banked memory

# Compilation

    zcc +gb -create-app [file.c]

A .gb file will be created suitable for insertion into an emulator

# Library support

The "standard" GBDK library is provided with z88dk. The standard library (printf, strings etc) is supplied by the usual z88dk libraries.

## Generic console

The following screen modes are supported by the generic console:

* "Text", mode 0, tile based, supports lores graphics and scrollowing
* "Drawing", mode 1, APA mode

## Limitations

* Graphics support is not complete

## Compiler support

At present only sccz80 is supported for creating gameboy ROMs.

# Links

* [Jeff Frohwein's pages](http://www.devrs.com/gb/) - historically the best gameboy development resource