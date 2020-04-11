**This page describes the ZX Next as used under the classic library. If you're creating software solely for the ZX Next then it's recommended that you use newlib. However, if you aim to create software that can be ported to many machines, carry on reading.**

# Hardware summary

* Z80n @ 3.54, 7, 14, 28Mhz
* 1,2 Mb RAM
* Audio: AY-3-8192 x3

_This port is currently being brought up and hasn't hit master/nightly builds yet_

## Classic library support (+zxn)

* [x] Native console output
* [x] Native console input
* [x] ANSI vt100 engine
* [x] Generic console
    * [x] Redefinable font
    * [x] UDG support
    * [x] Paper colour
    * [x] Ink colour
    * [x] Inverse attribute
    * [x] Bold attribute
    * [x] Underline attribute
* [ ] Lores graphics
* [x] Hires graphics
* [x] PSG sound
* [x] One bit sound
* [x] Inkey driver
* [x] Hardware joystick
* [x] File I/O
* [x] Interrupts
* [ ] RS232

# Compilation

    zcc +zxn program.c -lndos --hardware-keyboard -create-app

A .nex file will be created suitable for loading on the real hardware 

# Generic console screen modes

The generic console supports switching between the Timex screen modes:

* Mode 0 = Standard ZX Screen mode (256x192)
* Mode 1 = Uses Time Screen 1 (256x192)
* Mode 2 = Timex High colour screen (256x192)
* Mode 6 = Timex Hires screen (512x192)
* Mode 64 = Tilemap mode 40x32
* Mode 65 = Tilemap mode 40x32 (no attributes)
* Mode 66 = Tilemap mode 80x32
* Mode 67 = Tilemap mode 80x32 (no attributes)

The colours for the hires mode can modified by OR (|) the following values with the mode value:

| Value | Colour |
|-|-|
| 0x00 | Black on white |
| 0x08 | Blue on yellow |
| 0x10 | Red on cyan |
| 0x18 | Magenta on green |
| 0x20 | Green on magenta |
| 0x30 | Yellow on blue |
| 0x38 | White on black |

## Tilemap mode

When running in tilemap mode, z88dk uses the following address by default: Tile definitions = $4c00, tilemap = $6c00. However, the code reads the registers to determine the appropriate addresses so your application can repoint as necessary.



