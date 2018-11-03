# Machine Specifications

* CPU: Z80 @ 3.579545 Mhz
* VDP: TMS9928, 16k VRAM
* PSG: SN76489
* RAM: 64k 
* ROM: 32k

The Colecovision Adam is supported when running CP/M:

    zcc +cpm program.c -subtype=col1 -create-app

Will create a 40 track SSDD disk image that can be inserted as drive b: and your application started

## Extensions

Planned: TMS9928 libraries, Sound, gencon

## Emulator

Tested with the Mame emulator

## Links

