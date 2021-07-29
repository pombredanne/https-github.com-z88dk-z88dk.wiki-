# Machine Specifications

* CPU: Z80 @ 3.579545 Mhz
* VDP: TMS9928, 16k VRAM
* PSG: SN76489
* RAM: 64k 
* ROM: 32k


## Classic library support (`+coleco -subtype=adam`)

* [ ] Native console output
* [ ] Native console input
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
* [ ] One bit sound
* [ ] Inkey driver
* [x] Hardware joystick
* [ ] File I/O
* [x] Interrupts
* [ ] RS232

The Colecovision Adam supports the generation of EOS programs: 

    zcc +coleco program.c -subtype=adam -create-app

Will generate a ddp image that allows full use of the 64k RAM within the Adam.

Apart from keyboard reading, no EOS facilities are supported by z88dk at present.

### CP/M support

The Colecovision Adam is supported when running CP/M:

    zcc +cpm program.c -subtype=adam -create-app

Will create a 40 track SSDD disk image that can be inserted as drive b: and your application started

#### CP/M Extensions

- Generic console
- ANSI terminal
- MSX graphics library
- PSG support

#### VDP interrupt

On the Adam, the VDP interrupt is raised using NMI. As a result of address $66 falling within the CP/M FCB block, VDP interrupts are disabled on the Adam.

## Emulator

Tested with the Mame emulator and CPM 2.2 boot disk

## Links
