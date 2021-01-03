# Spectravideo SVI

## Classic library support (`+svi`)

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
* [x] Lores graphics
* [x] Hires graphics
* [x] PSG sound
* [ ] One bit sound
* [x] Inkey driver
* [x] Hardware joystick
* [ ] File I/O
* [x] Interrupts
* [x] RS232




## Quick start

    zcc  +svi -lm -create-app program.c

This will create a .CAS file.
Once compiled, run the SVI emulator and load the "tape" with the /r option.

If you are using the Jimmy Mårdell's ["SVI" emulator](http://www.acc.umu.se/~yarin/sviemu/), you can specify the autoload option:
    svi -tapa adv.cas

Other emulators, in example BlueMSX need the full BASIC command to be typed in:
    bload "cas:",r
 
A WAV audio file can be created too, with the following option:

    zcc  +svi -lm -create-app -subtype=wav program.c


### 16K model

To run on a 16K machine, you need to move the code origin to the upper half.
'-zorg=49200' should be a good option for you zcc command line.  Any working address for the 16k model will still work on the 32k models.

### ROM cartridge

A ROM cartridge can be generated with:

    zcc  +svi -lm -create-app -subtype=rom program.c

In this model, the firmware isn't available so to read the keyboard you must add the option `--hardware-keyboard` to redirect calls to the "inkey" keyboard reading routines.

### Autoboot floppy

An auto booting disk can be generated with:

    zcc  +svi -lm -create-app -subtype=disk program.c

The .svi image created is a raw image file that is understood by mame. You can load it in Mame with the following options:

    ./mame64 svi328 -exp sv601 -exp:sv601:1 sv801 -flop1 a.svi


## 80 Column mode with the SV-806 expansion card

The SV-806 card is supported by the generic console. In a similar manner to the Einstein, switch to mode 10 using `console_ioctl`

You'll need to start mame with the following options (this includes the floppy interface):

    ./mame64 svi328 -exp sv601 -exp:sv601:1 sv801 -exp:sv601:2 sv806

Note, in this mode the standard `fgetc_cons()` driver appears to not work. So you'll need to switch to the inkey driver using: `-pragma-redirect:fgetc_cons=fgetc_cons_inkey`

## Serial port with the SV-805 RS232 expansion card

The SV-805 card contains an 8250 UART and is supported using the z88dk libraries using the API defined in `<rs232.h>`.

For Mame usage, start with:

    ./mame64 svi328 -exp sv601 -exp:sv601:1 sv801 -exp:sv601:2 sv805 -exp:sv601:2:sv805:rs232 null_modem -bitb socket.localhost:25233 -flop1 a.svi

Which will boot an SVI floppy containing your application. You can connect to the serial port by telnetting to port 25233

## The TMS9918a library

z88dk provides a [TMS9918a](Classic-TMS9918) library that allows hardware access to the VDP chip. All graphical functionality supplied by z88dk uses these functions and such is hardware independent.
