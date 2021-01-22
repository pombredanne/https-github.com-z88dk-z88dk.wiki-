# Hardware summary

* 8080 @ 2 Mhz
* xxk RAM (64k assumed)
* Serial console only

## Classic library support (`+altair8800`)

* [x] Native console output
* [x] Native console input
* [ ] ANSI vt100 engine
* [x] Generic console (VTI S100 board)
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

# Supporting the VTI S100 board

Adding the options:

    --vti -pragma-export:VTI_DISPLAY=0xc000

Will enable support for the VTI display board. Change 0xc000 to match the addresses of dip switches on the hardware.

# Notes

In terms of hardware support, only the serial console is supported.
