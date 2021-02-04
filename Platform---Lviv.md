# Lviv/Lvov PK-01 Hardware summary

* MHB 8080A  @ 2.5Mhz
* 48k contiguous RAM
* 16k VRAM giving 256x256 2bpp resolution screen with 4/16 colours

## Classic library support (`+lviv`)

* [x] Native console output
* [x] Native console input
* [ ] ANSI vt100 engine
* [x] Generic console
    * [x] Redefinable font 
    * [x] UDG support
    * [x] Paper colour
    * [x] Ink colour
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


# Compilation

    zcc +lviv program.c -create-app

A .lvt file is created suitable for loading into an emulator.

# Keyboard

By default the keyboard layout is JCUKEN, should you wish you can switch to
a qwerty keymap with the additional option:

-pragma-redirect:in_keytranstbl=in_keytranstbl_qwerty

# Issues

* Mame seems to have issues loading larger programs
* Text printing is fairly slow

# Links

* [PK01 Emulator](https://github.com/izhaks/PK01LvovEmulator)
