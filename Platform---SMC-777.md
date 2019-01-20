# Hardware summary

* Z80 @ 4.028 Mhz
* 32k ROM (SMC-70) 16k ROM (SMC-777)
* 64k RAM
* MC6845 
* 32k VRAM
* SN76489 (SMC-777)
* 2 joysticks (port 0x051 and 0x151)

_This port is under development_

This machine can run both CP/M and S-OS and hence applications compiled for either OS will work. The target specific library can be used under either OS but testing has mostly taken place under CP/M.


## Classic library support

* [x] Native console output
* [x] Native console input
* [ ] ANSI vt100 engine
* [x] Generic console
    * [x] Redefinable font 
    * [x] UDG support
* [x] Lores graphics
* [ ] Hires graphics
* [x] PSG sound
* [ ] One bit sound
* [ ] Inkey driver
* [ ] Hardware joystick
* [x] File I/O

## Compilation

    zcc +cpm file.c -subtype=smc777 -create-app

This command will create a .dsk image suitable for use in emulators.

## Running compiled code

The target is a CP/M derivative, so you will need to boot into CP/M and launch the application from the secondary drive.

## Emulators

The target has been tested using the Takeda emulator.

## Graphics support

The following screen modes are available:

* Mode 0: 80x25, 160x50 lores graphics
* Mode 1: 40x25,  80x50 lores graphics

## Links

* [Takeda Emulator](http://takeda-toshiya.my.coocan.jp/smc777/index.html)
* [Technical details on z88dk issue](https://github.com/z88dk/z88dk/issues/1067)


