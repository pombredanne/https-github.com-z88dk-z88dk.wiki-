# Hardware summary

* 8080 @ 2 Mhz
* xxk RAM (64k assumed)
* Serial console only

## Classic library support (`+altair8800`)

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

_This target is very bare bones_

# Compilation

    zcc +altair8800 world.c -create-app

This will create a .rom file suitable for loading into an emulator.

# Adjusting the RAM size

By default, the target places writable variables at address 0xc000 and the stack at the top of memory at 0xfd00. To adjust these use the pragmas `-pragma-define:CRT_ORG_BSS=0xNNNN` and `-pragma-define:REGISTER_SP=0xNNNN`

# Notes

In terms of hardware support, only the serial console is supported.
