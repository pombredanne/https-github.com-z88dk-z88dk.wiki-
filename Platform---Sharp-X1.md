# Hardware Summary

* z80a @ 4MHz
* ROM: IPL (4KB) + chargen (2KB)
* RAM: 64kb RAM
* MC6845: VRAM: 4KB (text) + PCG (6KB) + GRAM (48KB)
* AY8910

This machine can run software natively (`+x1`), via CP/M (`+cpm -subtype=x1`) and SOS (`+sos`)

_This target is receiving some attention at the moment and options/features may not be in nightly builds_

## Classic library support (`+x1`)

* [ ] Native console output
* [x] Native console input
* [x] ANSI vt100 engine
* [x] Generic console
    * [x] Redefinable font 
    * [x] UDG support
* [ ] Lores graphics 
* [x] Hires graphics (320x200 + 640x200)
* [x] PSG sound
* [ ] One bit sound
* [x] Inkey driver
* [ ] Hardware joystick
* [ ] File I/O
* [ ] Interrupts
* [ ] RS232


# Quick start

    IM1 mode (org at address 0)
        zcc +x1 -create-app -lm adv_a.c

    IM2 mode (org address = 32768 or more)
        zcc +x1 -create-app -subtype=im2 -zorg=`<address>` -lm adv_a.c


An autoboot d88 disk image (.d88 extension) is created automatically by appmake.


## Generic console

The generic console supports the following screen modes:

* Mode 0: 40x25, graphics 320x200
* Mode 1: 80x25, graphics 640x200

### Custom fonts/udgs

The PCG ram is used for custom fonts and UDGs, the PCG ram can only be loaded when in mode 0.

Loading data into the PCG is horrendously slow, so it's recommended that your application present a "Please wait" style screen whilst this is happening. The library will automatically load any compile configured `CRT_FONT` without such a screen so it's recommended that fonts are configured using the `GENCON_SET_FONT32` ioctl.

# Emulators

* Mame
* Takeda

Software (including CP/M) is available in the Neo Kobe archives. 

# Links

* [x1center.org](http://www.x1center.org/) - technical information