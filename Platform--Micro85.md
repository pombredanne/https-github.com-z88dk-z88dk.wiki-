Hardware summary

Home brew hardware with:

* Intel 8085
* 32k ROM/32k RAM
* Support for additional components

## Classic library support (`+mikro80`)

* [x] Native console output
* [x] Native console input
* [ ] ANSI vt100 engine
* [ ] Generic console
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
* [ ] One bit sound
* [ ] Inkey driver
* [ ] Hardware joystick
* [ ] File I/O
* [ ] Interrupts
* [ ] RS232

# Compilation

    zcc +micro85 program.c -create-app

A .ihx file is produced suitable for burning to ROM.

## Links

* https://hackaday.io/project/176653-micro8085
