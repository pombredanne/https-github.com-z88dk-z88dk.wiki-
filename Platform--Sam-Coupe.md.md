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
* [x] PSG sound (etracker player available)
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

* `-subtype=allram` (default) Loads the program to A+B+C+D, screen paged on demand into C+D
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

Note: The examples within z88dk will need the screen mode setting initially before anything can be displayed on screen.

## Palette/CLUT mapping

The Sam Coupe has a user definable palette and thus palette mapping does not take place for gencon modes 2,3,4. The mapping that does take place or mode 1 may be incorrect if you have redefined the palette.

## Graphics support

Pixel based operations and primitive drawing is supported.

# Hardware support 

The file [<arch/sam.h>](https://github.com/z88dk/z88dk/blob/master/include/arch/sam.h) defines additional functions and features for controlling the hardware of the target.

## Interrupts

The line and frame interrupts are distinguished and ISRs can be registered for each trigger using the following two functions:

    #include <arch/sam.h>
    add_raster_int(func);
    sam_install_line_isr(func);

In the default allram subtype, NMI handlers can be registered with:

    #include <interrupt.h>
    nmi_install_isr(func)

## Audio support

The beeper is supported via the functions in `<games.h>`, etracker module supports is available via `<psg/etracker.h>`

## The stack

In the default `allram` subtype, an area for the stack is reserved in segments B+D, by default 1024 bytes are made available, this can increased or decreased using the option `-pragma-define:CRT_STACK_SIZE=nnnn` where `nnnn` is the required size.

# Real Hardware

You can use the *dskman* utility to save your files to a dsk-image which can be written out to a DD floppy and read by a real SAM Coupé.  
