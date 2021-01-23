# Hardware summary

* 8080 @ 2 Mhz
* xxk RAM (64k assumed)
* Serial console only

## Classic library support (`+altair8800`)

* [x] Native console output
* [x] Native console input
* [ ] ANSI vt100 engine
* [x] Generic console (VTI S100 board, VDM-1 board)
    * [ ] Redefinable font 
    * [ ] UDG support
    * [ ] Paper colour
    * [ ] Ink colour
    * [ ] Inverse attribute
    * [ ] Bold attribute
    * [ ] Underline attribute
* [x] Lores graphics (VTI and VDM-1)
* [ ] Hires graphics
* [ ] PSG sound
* [ ] One bit sound
* [ ] Inkey driver
* [ ] Hardware joystick
* [ ] File I/O
* [ ] Interrupts
* [ ] RS232

_This target is very bare bones._

This target also supports CP/M.

# Compilation

    zcc +altair8800 world.c -create-app

This will create a .rom file suitable for loading into an emulator.

# Adjusting the RAM size

By default, the target places writable variables at address 0xc000 and the stack at the top of memory at 0xfd00. To adjust these use the pragmas `-pragma-define:CRT_ORG_BSS=0xNNNN` and `-pragma-define:REGISTER_SP=0xNNNN`

# Video boards

## VTI S100 board

Adding the options:

    --vti -pragma-export:VTI_DISPLAY=0xc000

Will enable support for the VTI display board and 128x48 lores graphics. Change 0xc000 to match the addresses of dip switches on the hardware. You will have to adjust the address of the BSS section as detailed above if the board overlaps.

## VDM-1 board

Adding the option:

    -vdm

Will enable support for the VDM-1 display board and 64x16 super lores graphics. The memory mapped address of the board defaults to 0xcc00. Should your board be at a different address add the option `-pragma-export:VDM_DISPLAY=0xNNNN` where NNNN corresponds to the address. You will have to adjust the address of the BSS section as detailed above.

# Notes

In terms of hardware support, only the serial console is supported.
