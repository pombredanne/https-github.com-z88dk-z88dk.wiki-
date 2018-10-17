# Hardware summary

**Super80, Super80d, Super80e, Super80m**

* Z80 @ 2MHz (effective speed is 1MHz)
* 16k, 32k or 48k RAM (0000-BFFF)
* 12k ROM (C000-EFFF)
* 3.5k RAM (F000-FDFF), comes with the "64k ram" modification
* 0.5k RAM (FE00-FFFF) for Chipspeed colour board
* 16x32 character mapped at 0xbe00 by default

**Super80r, Super80v**

* Z80 @ 2MHz
* 16k, 32k or 48k RAM (0000-BFFF)
* 12k ROM (C000-EFFF)
* 2k Video RAM (F000-F7FF) banked with Colour RAM (modified Chipspeed board)
* 2k PCG RAM (F800-FFFF) banked with Character Generator ROM
* MC6845 on $10 and $11

*This family is not supported yet*

# Compilation

    zcc +super80 adv_a.c -create-app

A .ql file is produced suitable for the QuickLoad feature within Mame.

# Emulator

The port has been tested using Mame.

# Features

## Character set

All models have a different character set, only the ASCII range 32 - 95 is consistent across the machines. The Super80 doesn't have lower case, so all letters are in upper case.

## Colour support

Colour is supported on the super80m model. Mappings may not be to standard ANSI colour schemes.

## Graphics

Graphics support (at a resolution of 64x48) is only available for the Super80e


# Links

* [The Super 80 at Wikipedia](http://en.wikipedia.org/wiki/Dick_Smith_Super-80_Computer)
* [Hardware overview](http://interbutt.com/mess/super80/)
* [Super-80 Software](http://interbutt.com/mess/super80/super80_software.zip)
