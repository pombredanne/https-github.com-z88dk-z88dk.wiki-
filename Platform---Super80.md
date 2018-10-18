# Hardware summary

**Super-80, Super-80d, Super-80e, Super-80m**

* Z80 @ 2MHz (effective speed is 1MHz)
* 16k, 32k or 48k RAM (0000-BFFF)
* 12k ROM (C000-EFFF)
* 3.5k RAM (F000-FDFF), comes with the "64k ram" modification
* 0.5k RAM (FE00-FFFF) for Chipspeed colour board
* 16x32 character mapped at 0xbe00 by default

**Super-80r, Super-80v**

* Z80 @ 2MHz
* 16k, 32k or 48k RAM (0000-BFFF)
* 12k ROM (C000-EFFF)
* 2k Video RAM (F000-F7FF) banked with Colour RAM (modified Chipspeed board)
* 2k PCG RAM (F800-FFFF) banked with Character Generator ROM
* MC6845 on $10 and $11

# Compilation

Super80, Super80d, Super80e, Super80m:

    zcc +super80 adv_a.c -create-app

Super80r, Super80v:

    zcc +super80 adv_a.c -create-app -clib=vduem

A .ql file is produced suitable for the QuickLoad feature within Mame.

# Emulator

The port has been tested using Mame.

# Features

This family of computers has different display characteristics and capabilities on each model. The following table shows the features of the z88dk libraries when run on each machine.

 | Model | Console size | Colour | Lower case | Custom font | UDGs | Lores Graphics |
 | --- | --- | --- | --- | --- | --- | --- |
 Super-80 | 32x16 | N | N | N | N | N |
 Super-80d | 32x16 | N | Y | N | N | N |
 Super-80e | 32x16 | N | Y | N | N | 64x48 |
 Super-80m | 32x16 | Y | Y | N | N | 64x48 (alternate charset) |
 Super-80r | 80x25 | N | Y | Y | Y | 160x50 |
 Super-80v | 80x25 | Y | Y | Y | Y | 160x50 |

## Custom font and UDGs

The font size on the Super-80r and Super-80v is 7x9. However, for compatibility with the other targets, z88dk will pad the specified 8x8 font out with a blank line at the top of each character. The machine will cut off the right-most bit of each character when displaying it.

As a result of this geometry, the lores graphics on these machines is slightly mis-shaped.

# Links

* [The Super 80 at Wikipedia](http://en.wikipedia.org/wiki/Dick_Smith_Super-80_Computer)
* [Hardware overview](http://interbutt.com/mess/super80/)
* [Super-80 Software](http://interbutt.com/mess/super80/super80_software.zip)