# Hardware summary

* Z80 clone
* 16 - 64k memory
* 64x32 character display

## Classic library support (`+homelab`)

* [ ] Native console output
* [ ] Native console input
* [ ] ANSI vt100 engine
* [x] Generic console
    * [ ] Redefinable font 
    * [ ] UDG support
    * [ ] Paper colour
    * [ ] Ink colour (Colour Ace only)
    * [ ] Inverse attribute
    * [ ] Bold attribute
    * [ ] Underline attribute
* [x] Lores graphics
* [ ] Hires graphics (only plot)
* [ ] PSG sound
* [ ] One bit sound
* [x] Inkey driver
* [ ] Hardware joystick
* [ ] File I/O
* [ ] Interrupts
* [ ] RS232

# Compilation

    zcc +homelab program.c -create-app

A .htp file will be generated suitable for loading into Mame or Homelab emulator running in Homelab 4 mode.

Once loaded (via quick load in mame or autoload in Homelab.exe) enter the following command:

    CALL $4300

# Library notes

The font is fixed in the machine ROM and switches out symbols such as square and curly brackets for accented characters.

# Links

* [Information](http://homelab.8bit.hu/)