# Sam Coupe 

![](images/platform/sam.jpg)

# Hardware

* Z80B @ 6Mhz
* 256/512kb RAM + extensions
* SAA1099 sound generator


## Classic library support (`+sam`)

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
* [ ] PSG sound
* [x] One bit sound
* [x] Inkey driver
* [x] Hardware joystick
* [ ] File I/O
* [x] Interrupts
* [ ] RS232

_This platform is currently getting some long overdue love, the state of the repo and this page may be out of sync_


# Quick start

    zcc +sam -lm application.c -create-app

A .MGT disc image will be created suitable for insertion into an emulator
or writing to a physical disc. The disc will autoboot and the application will
start automatically.

The process will automatically insert a version of SAMDOS onto the disc, to override
it with an alternate DOS file the `-Cz--dosfile option can be used`:

    zcc +sam -lm application.c -create-app -Cz--dosfile=/path/to/dosfile


# Memory models

* `-subtype=highram` (default) Loads the program to C+D, screen into A+B
* `-subtype=basic` Uses MODE 1 from a BASIC environment (this subtype has much reduced functionality)

# Screen modes

The Sam Coupe hardware provides the following screen modes:

| Screen Mode | Colours | Text Resolution | Graphics Resolution | Memory Used | Notes |
|-|-|-|-|-|-|
| 1 | 8 | 32x24/64x32 | 256x192 | 6.75k | Lowers the effect clock speed to ~3.5MHz. Colour resolution is 32x24 |
| 2 | 8 | 32x24 | 256x192 | 12k | Colour resolution is 32x192 |
| 3 | 4 | 64x24 | 512x192 | 24k | Colour resolution is 512x192 |
| 4 | 16| 32x24 | 256x192 | 24k | Colour resolution is 256x192 |

All 4 screen modes on the SAM are supported by z88dk, to switch between them
use the following code:

    int  mode = 2;
    console_ioctl(IOCTL_GENCON_SET_MODE,&mode);

The screen modes are presented as they are for SAM Basic, that is
with mode=1 representing the +zx compatible mode and 4 being the
high colour screen mode.

_An alternative method of setting the screen mode will be added_

# Graphics support

Pixel based operations and primitive drawing is supported.

# Emulator notes

Use the [SimCoupé](http://www.simcoupe.org/) emulator or the [latest development builds](https://github.com/simonowen/simcoupe), press F4 and you can import any binary in the file system to any given BASIC address of the SAM Coupé.  For example, use 32768 as the import address and when returning to BASIC you can just CALL 32768 to run the code.


# Real Hardware

You can use the *dskman* utility to save your files to a dsk-image which can be written out to a DD floppy and read by a real SAM Coupé.  
