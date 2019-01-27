# Hardware summary

* Z80 @ 4 Mhz
* 48k ROM 
* 64k RAM
* MC6845 
* 56k/64k? VRAM
* 2x SN76489 (SMC-777)
* 2 joysticks

_This target is under development_

There is a very little downloadable software for this machine. In theory it can run both CP/M and S-OS and hence applications compiled for either OS will work. However we've been unable to find versions of either OS that can be used on this machine. As such usage under those targets is complete untested.

As a result the machine has been tested under the `+pasiopia7` target.

## Classic library support

* [ ] Native console output
* [ ] Native console input
* [ ] ANSI vt100 engine
* [x] Generic console
    * [ ] Redefinable font 
    * [ ] UDG support
* [x] Lores graphics
* [ ] Hires graphics
* [ ] PSG sound
* [ ] One bit sound
* [x] Inkey driver
* [x] Hardware joystick
* [ ] File I/O
* [ ] Interrupts

## Compilation

   zcc +pasopia7 app.c -create-app

A .dsk file will be created that can be inserted into an emulator and will auto boot.

The system runs in all RAM mode, meaning that a full 64k is available to your application. This operational mode causes problems with the Takeda emulator and the reset menu option doesn't work as expected.

## Emulators

The target has been tested using the Takeda emulator. The Mame emulator implementation is incomplete and does not support floppy discs.

## Graphics support

The following screen modes are available:

* Mode 0: 80x25, 80x25 lores graphics
* Mode 1: 40x25, 40x25 lores graphics

The machine does support hires graphics, but it is not currently supported by z88dk.

## Links

* [Takeda Emulator](http://takeda-toshiya.my.coocan.jp/index.html)
* [Some technical info and experiments](https://pasopia700.blogspot.com)