# Hardware summary

* U880  @ 2.048Mhz
* 16k RAM (A/B-32), 32k RAM (A/B-48), 48k RAM (A/B/64)

## Classic library support (`+primo`)

* [ ] Native console output
* [ ] Native console input
* [ ] ANSI vt100 engine
* [x] Generic console
    * [x] Redefinable font 
    * [x] UDG support
    * [ ] Paper colour
    * [ ] Ink colour (Colour Ace only)
    * [x] Inverse attribute
    * [x] Bold attribute
    * [x] Underline attribute
* [ ] Lores graphics
* [x] Hires graphics (only plot)
* [ ] PSG sound
* [x] One bit sound
* [x] Inkey driver
* [ ] Hardware joystick
* [ ] File I/O
* [x] Interrupts
* [ ] RS232

# Compilation

    zcc +primo program.c -create-app

A .pp file will be generated suitable for loading into Mame.

# Keyboard and input

An inkey keyboard handler is supplied supporting concurrent keypresses. At present the keyboard mappings may not be correct.

# Links

* http://primo.homeserver.hu
