# Machine Specifications

* CPU: Z80 @ 3.5Mhz
* VDP: TMS992x, 16k VRAM
* PSG: AY-3-891x
* RAM: 32k
* ROM: 32k

The 64k model is also supported.

## Classic library support (`+lm80c`)

* [x] Native console output
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

The LM80C ROM is under constant development, with the result that addresses of ROM routines used by z88dk change between releases. To address this, the `-clib=` option is used to target the firmware version. To view the supported -clib options, run the command:

    zcc +lm80c

 
# Support

The target utilises the firmware printing by default. For finer grained control add the `--generic-console` option to switch to z88dk's generic console. When using it, you will need to select the screen mode prior to printing. For example to switch to mode 2, insert the following lines towards the start of your program:

    int mode = 2;
    console_ioctl(IOCTL_GENCON_SET_MODE, &mode);

The VDP used by the LM-80C is the same as the one in the MSX, as such many MSX examples will work. 

The WYZ and VT2 PSG engines are supported by this target.


# Keyboard

The keyboard is supported by utilising the RAM addresses set by the firmware.

# Links

* [Online emulator](https://nippur72.github.io/lm80c-emu/)
* [Emulator source](https://github.com/nippur72/lm80c-emu)
* [Original project](https://github.com/leomil72/LM80C)
