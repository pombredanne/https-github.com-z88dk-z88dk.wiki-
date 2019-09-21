# Hardware

* Sharp LR35902 at 4.19 MHz
* 8k VRAM
* 8k General purpose RAM
* 32k cartridge

# Compilation

    zcc +gb -create-app [file.c]

A .gb file will be created suitable for insertion into an emulator

# Library support

The "standard" GBDK library is provided with z88dk. The standard library is supplied by the usual z88dk libraries.

## Compiler support

At present only sccz80 is supported for creating gameboy ROMs.

# Links

* [Jeff Frohwein's pages](http://www.devrs.com/gb/) - historically the best gameboy development resource