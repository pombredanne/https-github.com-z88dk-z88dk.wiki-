# Hardware summary

* Z80 @ 4 Mhz
* 64k RAM, 24k ROM + 4k chargen
* HD46505SP (aka mc6845) as CRTC

# Compilation

    zcc +alphatro world.c -create-app

The resulting .rom file can be loaded into mame and run as follows:

    mame64 alphatro -cartridge1 a.rom

# Generic console and graphics modes

The Alphatronic PC supports 40 and 80 column modes and the corresponding lores graphical. You can switch modes using `console_ioctl()` with the following mode values:

* Mode 0: 40x24 text, 80x48 graphics
* Mode 1: 80x24 text, 160x48 graphics

Colour is supported in both modes.

# Screenshots

Running the VT52/generic console test program:

![](images/platform/alphatro_ansivt.png)


# Limitations

* Although the machine has 64k RAM, only 16k cartridges can be generated until we can create disc images

# Links

* [Manual](http://www.retroarchive.org/hardware/Royal/royal_alphatronic_manual.pdf) - appendices have tech details
* [More technical details](http://81.105.120.101/alphatronic/)
* [Mame Driver](https://github.com/mamedev/mame/blob/master/src/mame/drivers/alphatro.cpp)
* [Thread on bannister.org](https://forums.bannister.org/ubbthreads.php?ubb=showflat&Number=75540&page=1)