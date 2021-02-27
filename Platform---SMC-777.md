# Hardware summary

* Z80 @ 4.028 Mhz
* 32k ROM (SMC-70) 16k ROM (SMC-777)
* 64k RAM
* MC6845 
* 32k VRAM
* SN76489 (SMC-777)
* 2 joysticks (port 0x051 and 0x151)

_This port is under development_

As well as producing native binaries, this machine can run both CP/M and S-OS and hence applications compiled for either OS will work. The target specific library can be used under either OS but testing has mostly taken place under CP/M.


## Classic library support

* [x] Native console output
* [x] Native console input
* [ ] ANSI vt100 engine
* [x] Generic console
    * [x] Redefinable font 
    * [x] UDG support
* [x] Lores graphics
* [x] Hires graphics
* [x] PSG sound
* [ ] One bit sound
* [ ] Inkey driver
* [x] Hardware joystick
* [x] File I/O (CP/M)
* [ ] Interrupts
* [ ] RS232

## Compilation

    zcc +smc777 file.c -create-app

Will generate a disc that can be booted by the IPL. In this mode, the application can use all the memory within the machine.

Generating a CP/M binary provides more functionality such as file handling:

    zcc +cpm file.c -subtype=smc777 -create-app

This command will create a .dsk image suitable for use in emulators.

## Emulators

The target has been tested using the Takeda emulator.

## Graphics support

The following screen modes are available:

* Mode 0: 80x25, 160x50 lores graphics
* Mode 1: 40x25,  80x50 lores graphics
* Mode 4: 80x25, 320x200 hires graphics
* Mode 5: 40x25, 320x200 hires graphics
* Mode 8: 80x25, 640x200 hires graphics
* Mode 9: 40x25, 640x200 hires graphics

The hires graphics plane is separate to the text plane. As a result graphics are overlaid by any text printed. The background colour of the hires graphics plane can be configured using the `textbackground()` function. 

## Links

* [Takeda Emulator](http://takeda-toshiya.my.coocan.jp/smc777/index.html)
* [Technical details on z88dk issue](https://github.com/z88dk/z88dk/issues/1067)

