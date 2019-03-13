# Hardware summary

* Z80 @ 4 Mhz
* 64k RAM
* MC6845 with 24k VRAM
* Boots to BASIC or CP/M

_This machine is currently being brought up_

## Classic library support

* [ ] Native console output
* [ ] Native console input
* [ ] ANSI vt100 engine
* [x] Generic console
    * [ ] Redefinable font 
    * [ ] UDG support
* [ ] Lores graphics (32x16)
* [ ] Hires graphics (256x96)
* [ ] PSG sound
* [ ] One bit sound
* [ ] Inkey driver
* [ ] Hardware joystick
* [ ] File I/O
* [ ] Interrupts
* [ ] RS232

# Compilation

    zcc +cpm -subtype=excali64 program.c

Will create a raw disc image suitable for use by CP/M

# Screen modes

* Mode 0: 80x25 
* Mode 1: 40x25

# Links
