**The Genius Leader is also supported under newlib using the +vgl target identifier.**

# Hardware summary

Genius Leader 2000/4000

* Z80 @ 4MHz
* 8k RAM
* 16k cartridge slot
* 2x20 HD44780 display (GL2000)
* 4x20 HD44780 display (GL4000)

Genius Leader 6000sl

* Z80 @ 4MHz
* 8k RAM
* 16k cartridge slot (other sizes are supported by hardware)
* 240x100px LCD


## Classic library support (`+gl -clib=gl2000`, `+gl -clib=gl4000`) (2000/4000)

* [ ] Native console output
* [x] Native console input
* [ ] ANSI vt100 engine
* [x] Generic console (default)
    * [ ] Redefinable font 
    * [ ] UDG support
    * [ ] Paper colour
    * [ ] Ink colour
    * [ ] Inverse attribute
    * [ ] Bold attribute
    * [ ] Underline attribute
* [ ] Lores graphics
* [ ] Hires graphics
* [ ] PSG sound
* [x] One bit sound
* [ ] Inkey driver
* [ ] Hardware joystick
* [ ] File I/O
* [ ] Interrupts
* [ ] RS232

## Classic library support (`+gl -clib=gl6000sl`) (6000sl)

* [ ] Native console output
* [x] Native console input
* [ ] ANSI vt100 engine
* [x] Generic console (default)
    * [x] Redefinable font 
    * [x] UDG support
    * [ ] Paper colour
    * [ ] Ink colour
    * [x] Inverse attribute
    * [x] Bold attribute
    * [x] Underline attribute
* [ ] Lores graphics
* [x] Hires graphics
* [ ] PSG sound
* [ ] One bit sound
* [x] Inkey driver
* [ ] Hardware joystick
* [ ] File I/O
* [ ] Interrupts
* [ ] RS232

_These targets are currently being brought up_

# Compilation

    zcc +gl -clib=gl2000 program.c -create-app

A .rom file will be created suitable for loading into the Mame gl2000 target.

    zcc +gl -clib=gl4000 program.c -create-app

A .rom file will be created suitable for loading into the Mame gl4000 target.

    zcc +gl -clib=gl6000sl program.c -create-app

A .rom file will be created suitable for loading into the Mame gl6000sl target.
