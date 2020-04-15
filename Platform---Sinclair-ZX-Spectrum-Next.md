**This page describes the ZX Next as used under the classic library. If you're creating software solely for the ZX Next then it's recommended that you use newlib. However, if you aim to create software that can be ported to many machines, carry on reading.**

# Hardware summary

* Z80n @ 3.54, 7, 14, 28Mhz
* 1,2 Mb RAM
* Audio: AY-3-8192 x3


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
* [x] Lores graphics
* [x] Hires graphics
* [x] PSG sound
* [x] One bit sound
* [x] Inkey driver
* [x] Hardware joystick
* [x] File I/O
* [x] Interrupts
* [ ] RS232

The classic `+zxn` port is based on a merge of the `+zx` and `+ts2068` ports with some extensions to support the new hardware. It's still under development and more features will be added.

# Compilation (RAM programs)

    zcc +zxn program.c -clib=classic -lndos -create-app -subtype=nex

A .nex file will be created suitable for loading on the real hardware or in an emulator.

# Compilation (DOT commands)

    zcc +zxn program.c -clib=classic -lndos -create-app -subtype=dot -o program

The file PROGRAM will be created which is a dot command suitable for launching from the Command Line in NextOS. DOT commands are limited to 8k in size.

# File IO

File operations are available via `<stdio.h>` (so `fopen()` and friends) and `<fcntl.h>` (so `open()`) by linking with the `-lesxdos` library.

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

z88dk supports the Next tilemap screen mode. When using this screen mode the entire display is taken over and the following tilemap addresses are used: Tile definitions = $4c00, tilemap = $6c00. On startup, the palette is redefined to support the screen colour defined in `<conio.h>`.

When the tilemap is configured with attributes (i.e mode 64, 66) then changing the ink colour, for example using  `textcolor()` will result in the character being printed in that colour.

_Note:_ The font isn't automatically copied when switching to tilemap modes - you will need to do this programatically using `console_ioctl()` with the argument `IOCTL_GENCON_SET_FONT32`.

When using the `IOCTL_GENCON_SET_FONT32` and `IOCTL_GENCON_SET_UDGS` ioctl values, the 8x8 characters are copied to the tile definitions in monochrome using colour indices of 0 for background and `CLIB_TILES_PALETTE_SET_INDEX` (default value 1) as set pixels.

The console driver supports your application changing the addresses for the tile definitions and tilemaps.

## Graphics

The classic graphics operations are supported in all available screen modes. In tilemap mode you must call `zxn_create_tilemap_graphics()` from `<arch/zxn.h>` to setup the lores graphics set prior to plotting anything.