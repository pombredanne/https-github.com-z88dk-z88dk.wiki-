# Hardware Summary

It's complicated, see https://en.wikipedia.org/wiki/PC-8800_series for details

## Classic library support

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
* [x] Lores graphics
* [x] Hires graphics
* [x] PSG sound
    * [x] WYZ Player
    * [ ] Vortex tracker
* [x] One bit sound
* [ ] Inkey driver
* [ ] Hardware joystick
* [ ] File I/O
* [x] Interrupts
* [ ] RS232


# Quick start

    zcc +pc88 -create-app -Cz--audio program.c

This will generate three files, **a.bin** (a pure binary block), **a.t88** (a file valid on many emulators), and **a.wav**, never tested but meant to be loaded on the real hardware.

    zcc +pc88 -subtype=disk program.c -create-app

This will create a .d88 image suitable for booting in FD1 on a PC88 emulator. With this model, allram mode is used, resulting in about 48k of usable space. As a downside, the firmware is not available.

# Generic console modes

The screen mode can be switched using the `console_ioctl()` function. The following modes are supported:

* Mode 0: 80x25, 640x200 graphics
* Mode 1: 40x25, 640x200 graphics
* Mode 2: 80x25 (bitmap characters), 640x200 graphics
* Mode 32: 80x25, 160x100 semigraphics
* Mode 33: 40x25, broken graphics
* Mode 34: 80x25 (bitmap characters), 160x100 semigraphics

The targets starts up in mode 0. It's recommended that mode 2 is used for applications: it provides more functionality and is hardware accelerated so is actually faster.

## Text Modes

Mode 0 (32) and 1 (33) support setting attributes for both colour and for character decoration. However, due to the way the attributes are handled on the PC8800 it does feel slower than text mode on other targets.

## Graphics modes

The PC-8800 target supports two different graphic modes:

- 640x200 multicolour pixels
- 160x100 semi-graphics.

As a result of the text attribute handling, text based graphics are slower than that implemented on other targets. As such it is recommended that multicolour graphics is used: as a result it is the default graphics mode. 

The multicolour graphics library by default takes advantage of the ALU and as such is "hardware accelerated", to disable the acceleration use the option `-clib=v1` or `-pragma-define:CLIB_PC8800_V2_ENABLED=0`.

# Advanced library usage

Target specific extras are listed in the <pc88.h> header file along with `__sfr` and documentation for the PC88 hardware.

# Emulator notes

An emulator capable of loading the T88 cassette file format is necessary, the default ORG address is $8A00, it was chosen peeping into the original software written in the eighties.

To run an appmake created program, first start N88 BASIC (on the M88 emulator the cpu clock must be set to 8mhz) or N80 BASIC, get to the prompt (answering first to the question about the "files?" number), then:

    MON
    R   (or L if you are using the N80 BASIC)
    G8A00

(CTRL-b would return to BASIC from monitor)


## M88 emulator

Please note that the M88 emulator and possibly also others require the correct CPU speed setting (usually 4 or 8 Mhz) to load correctly the program from the tape file.

# Appmake extras

The audio file can be slightly accelerated with the -Cz--fast build option.

Appmake can be also run in 'dumb mode' to convert any T88 file to wav:

    appmake -b program.t88 --audio --fast --dumb

# Links

[PC-8801 on Wikipedia](http://en.wikipedia.org/wiki/PC-8801)

https://web.archive.org/web/20190130002453/http://www.geocities.jp/retro_zzz/machines/nec/index.html

