# Вектор-06Ц - Vector06c Hardware summary

* MHB 8080A  @ ~3Mhz
* 64kk contiguous RAM
* 32k VRAM giving 256x256 resolution screen with 16 colours.

## Classic library support (`+vector06c`)

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
* [x] Hires graphics
* [ ] PSG sound
* [ ] One bit sound
* [x] Inkey driver
* [ ] Hardware joystick
* [ ] File I/O
* [x] Interrupts
* [ ] RS232


# Compilation

    zcc +vector06c program.c -create-app

A rom file is created suitable for loading into the Virtual Vector emulator.

# Links

* [Virtual Vector](http://sensi.org/~svo/virtualvector/)
* [Software](http://sensi.org/scalar/categories/) - downloads including CP/M