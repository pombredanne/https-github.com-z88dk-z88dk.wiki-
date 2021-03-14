# Специалист - Specialist Hardware summary

* MHB 8080A  @ ~2Mhz
* 36k contiguous RAM
* 12k VRAM giving 384 x 256 resolution

## Classic library support (`+special`)

* [x] Native console output
* [x] Native console input
* [ ] ANSI vt100 engine
* [x] Generic console
    * [x] Redefinable font 
    * [x] UDG support
    * [ ] Paper colour
    * [x] Ink colour (8 bit colour mod)
    * [x] Inverse attribute
    * [x] Bold attribute
    * [x] Underline attribute
* [ ] Lores graphics
* [x] Hires graphics
* [ ] PSG sound
* [ ] One bit sound
* [ ] Inkey driver
* [ ] Hardware joystick
* [ ] File I/O
* [ ] Interrupts
* [ ] RS232

_This target is incomplete_

# Compilation

    zcc +special program.c -create-app

A rks file is created suitable for loading into the Emu80 emulator.

The file can be loaded into Emu80 by issuing the following command:

    R

Select the file to load and wait.

Once loaded, some information will be displayed on screen showing the
start address, end address and checksum. Once that is displayed, start
the program by entering:

    G0000

# Links

* [Emu80](http://emu80.org) - information and emulators

* [Erik, a Z80 based clone of the "Specialist"] (http://micko-wip.blogspot.it/2008_03_01_archive.html)
