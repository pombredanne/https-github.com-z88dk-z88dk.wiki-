# Hardware summary

* Z80 @ 4 Mhz
* 64k RAM
* Intel 8275 CRT controller
* Boots to CP/M


## Classic library support

* [x] Native console output (CP/M)
* [x] Native console input (CP/M)
* [ ] ANSI vt100 engine
* [x] Generic console
    * [ ] Redefinable font 
    * [ ] UDG support
    * [ ] Paper colour
    * [ ] Ink colour
    * [ ] Inverse attribute
    * [ ] Bold attribute
    * [ ] Underline attribute
* [x] Lores graphics (80x25)
* [ ] Hires graphics
* [ ] PSG sound
* [ ] One bit sound
* [ ] Inkey driver
* [ ] Hardware joystick
* [x] File I/O (CP/M)
* [ ] Interrupts
* [ ] RS232

# Compilation

    zcc +cpm -subtype=rc700 program.c

Will create a raw disc image suitable for use by CP/M

# Library support

## Lores graphics

The lores graphics uses an incomplete character to represent a pixel which results in pixels not actually adjoining. Although the RC700 has onboard lores graphics, they are sized such that pixels are not evenly sized, as a result they aren't used by the z88dk driver.

# Links

* [Resources and emulator](http://www.jbox.dk/rc702/index.shtm)
