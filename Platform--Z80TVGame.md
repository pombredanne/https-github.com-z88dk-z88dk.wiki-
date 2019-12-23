# Hardware summary

* Z80 @ 4 Mhz
* 16k RAM, 8k VRAM
* 32k cartridge supports
* Single joystick
* 1 bit sound
* Custom B&W graphics 

## Classic library support (`+z80tvgame`)

* [ ] Native console output
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
* [ ] Inkey driver
* [x] Hardware joystick
* [ ] File I/O
* [x] Interrupts
* [ ] RS232


# Compilation

    zcc +z80tvgame file.c -create-app

A .rom file is created suitable for loading into an emulator

# Background

This target is a home-brew console designed and built by Mr Isizu. 

# Links

* [Takeda Emulator](http://takeda-toshiya.my.coocan.jp/z80tvgame/index.html)
* [Specification](http://w01.tp1.jp/~a571632211/z80tvgame/index.html)
