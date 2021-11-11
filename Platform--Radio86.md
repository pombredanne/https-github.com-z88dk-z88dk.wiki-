# Hardware summary

* К580ВМ80А @1.77Mhz
* Upto 32k RAM
* 2k firmware ROM

## Classic library support (`+radio86`)

* [x] Native console output
* [x] Native console input
* [ ] ANSI vt100 engine
* [x] Generic console
    * [ ] Redefinable font 
    * [ ] UDG support
    * [ ] Paper colour
    * [ ] Ink colour
    * [ ] Inverse attribute
    * [ ] Bold attribute
    * [ ] Underline attribute
* [x] Lores graphics
* [ ] Hires graphics
* [ ] PSG sound
* [ ] One bit sound
* [ ] Inkey driver
* [ ] Hardware joystick
* [ ] File I/O
* [ ] Interrupts
* [ ] RS232


# Compilation

    zcc +radio86 world.c -create-app

This will create a `.rkr` file suitable for loading into Emu80.

## Mame usage

Mame requires a slightly different file format, so programs need to be compiled
with an option indicating so:

    zcc +radio86 world.c -create-app -subtype=mame

To load into Mame issue the following commands:

```
I
G0000
```

# Library facilities

The Radio86 was a build-at-home computer and such there's a large number of
variants with different configurations. As a result, the z88dk port uses
the firmware to interact with the hardware. This has the following
limitations:

* Keypresses beep
* Printing is slower than it should be for a character mapped display
* The software cursors cannot be disabled
* Scrolling occurs when the bottom right hand corner is printed in

## Graphics

The default Radio86 character set provides 15 of the 16 characters needed to
provide a 2x2 semi-graphics grid: the missing character is probably masked
by the "beep" character code. As a result only very lores graphics (64x24) is
supported and the display is substandard due to the inter-row display gap.

# Links

* Monitor entry points docs: https://github.com/skiselev/radio-86rk
* Emu80
* Mame
