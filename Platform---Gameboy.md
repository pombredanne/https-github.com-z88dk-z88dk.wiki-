# Hardware

* Sharp LR35902 at 4.19 MHz
* 8k VRAM
* 8k General purpose RAM
* 32k cartridge

_This machine is being brought up_

# Compilation

    zcc +gb -create-app [file.c]

A .gb file will be created suitable for insertion into an emulator

# Library support

The "standard" GBDK library is provided with z88dk. The standard library (printf, strings etc) is supplied by the usual z88dk libraries.

## Limitations

* Graphics functions do not utilise the regular classic library
* Character input doesn't work

## Compiler support

At present only sccz80 is supported for creating gameboy ROMs.

# Links

* [Jeff Frohwein's pages](http://www.devrs.com/gb/) - historically the best gameboy development resource