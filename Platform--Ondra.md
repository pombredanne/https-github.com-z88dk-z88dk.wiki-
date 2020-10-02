# Hardware summary

* U880D @ 2Mhz
* 64k RAM, 4k ROM  

## Classic library support (`+ondra`)

* [x] Native console output
* [ ] Native console input
* [ ] ANSI vt100 engine
* [x] Generic console
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

_This target is in the process of being brought up_

# Compilation

    zcc +ondra world.c -create-app

This will create a .wav file suitable for loading into Mame. A .tap file is also created that should be readable by other emulators.

# Mame usage

To load a tape, press return and then hit play on the tape

# Ondra Keyboard

The Ondra keyboard consists of 3 rows of keys, the numbers are obtained by a modifier key. The mappings for z88dk/mame are:

* No modifier: Lower case letters
* Left shift: Upper case letters (UPPER key)
* Right shift: Symbols (SYMBOL key)
* Right Alt: Number (NUMBER key)
* Left Alt: Control (CS key)

# Notes

Drawing on the screen on this target is notoriously slow.

# Links

* https://sites.google.com/site/ondraspo186/