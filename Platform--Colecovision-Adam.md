# Machine Specifications

* CPU: Z80 @ 3.579545 Mhz
* VDP: TMS9928, 16k VRAM
* PSG: SN76489
* RAM: 64k 
* ROM: 32k

The Colecovision Adam is supported when running CP/M:

    zcc +cpm program.c -subtype=adam -create-app

Will create a 40 track SSDD disk image that can be inserted as drive b: and your application started

## Extensions

- Generic console
- ANSI terminal
- MSX graphics library
- PSG support

## Emulator

Tested with the Mame emulator and CPM 2.2 boot disk

## Links

