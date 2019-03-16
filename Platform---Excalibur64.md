# Hardware summary

* Z80 @ 4 Mhz
* 64k RAM
* MC6845 with 24k VRAM
* Boots to BASIC or CP/M

_This machine is currently being brought up_

## Classic library support

* [x] Native console output (CP/M)
* [x] Native console input (CP/M)
* [ ] ANSI vt100 engine
* [x] Generic console
    * [x] Redefinable font 
    * [x] UDG support
* [ ] Lores graphics
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

Will create a raw disc image suitable for use by CP/M

# Screen modes

* Mode 0: 80x25 (text)
* Mode 1: 40x25 (text)

# Links
