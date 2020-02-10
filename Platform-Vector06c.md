# Вектор-06Ц - Vector06c Hardware summary

* MHB 8080A  @ 2.048Mhz
* 32k contiguous RAM
* 16k VRAM giving 288x256 resolution screen.

## Classic library support (`+pmd85`)

* [ ] Native console output
* [ ] Native console input
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
* [ ] Hires graphics
* [ ] PSG sound
* [ ] One bit sound
* [x] Inkey driver
* [ ] Hardware joystick
* [ ] File I/O
* [x] Interrupts
* [ ] RS232

_This target is currently being brought up_

# Compilation

    zcc +vector06c program.c -create-app

A rom file is created suitable for loading into the Virtual Vector emulator.

# Links
