# Machine Specifications

* CPU: Z80 @ 3.5Mhz
* VDP: TMS992x, 16k VRAM
* PSG: AY-3-891x
* RAM: 32k
* ROM: 32k

## Classic library support (`+lm80c`)

* [ ] Native console output
* [x] Native console input
* [x] ANSI vt100 engine
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
* [x] PSG sound
* [ ] One bit sound
* [ ] Inkey driver
* [ ] Hardware joystick
* [ ] File I/O
* [ ] Interrupts
* [x] RS232

_This target is still being brought up_

# Compilation

    zcc +lm80c world.c -create-app

A .prg file will be generated which can be dropped onto the emulator.

# Support

If your application does any screen printing then you will need to change the screen mode prior to printing. For example to switch to mode 2, insert the following lines towards the start of your program:

    int mode = 2;
    console_ioctl(IOCTL_GENCON_SET_MODE, &mode);

The VDP used by the LM-80C is the same as the one in the MSX, as such many MSX examples will work. 

The WYZ and VT2 PSG engines are supported by this target.


# Keyboard

The keyboard is supported by utilising the RAM addresses set by the firmware. At the moment the key-repeat is somewhat faster than desired.

# Links

* [Online emulator](https://nippur72.github.io/lm80c-emu/)
* [Emulator source](https://github.com/nippur72/lm80c-emu)
* [Original project](https://github.com/leomil72/LM80C)
