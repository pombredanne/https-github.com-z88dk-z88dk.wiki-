# Hardware summary

* Z80 @ 4 Mhz
* 64k RAM
* MC6845 with 30k VRAM
* Boots to BASIC or CP/M

_This machine is currently being brought up_

## Classic library support

* [x] Native console output (CP/M)
* [x] Native console input (CP/M)
* [ ] ANSI vt100 engine
* [x] Generic console
    * [x] Redefinable font 
    * [x] UDG support
* [x] Lores graphics
* [ ] Hires graphics
* [ ] PSG sound
* [ ] One bit sound
* [ ] Inkey driver
* [ ] Hardware joystick
* [x] File I/O (CP/M)
* [ ] Interrupts
* [ ] RS232

# Compilation

    zcc +cpm -subtype=excali64 program.c

Will create a raw disc image suitable for use by CP/M (tested using Mame)

# Screen modes

* Mode 0: 80x24 (text) - 160x72 lores graphics
* Mode 1: 40x24 (text) - 80x72 lores graphics

## Custom fonts

In the text screen modes, the native font resolution is 8x12, all z88dk fonts are 8x8 and as such when they are used with the Excalibur they will be padded by 2 pixels at the top and bottom.

## PCG Banks

The console driver assigns PCG bank 0 to the font and UDGs. PCG bank 1 (characters 0-63) to lores graphics.

# Links
