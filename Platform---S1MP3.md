# Hardware summary

* ATJ2085 @ 25 Mhz?
* 16k RAM
* Samsung s6b0724 LCD screen: 32x132 pixels 

_This port is under development_

## Classic library support

* [x] Native console output
* [x] Native console input
* [ ] ANSI vt100 engine
* [x] Generic console
    * [x] Redefinable font 
    * [x] UDG support
* [ ] Lores graphics
* [ ] Hires graphics
* [ ] PSG sound
* [ ] One bit sound
* [ ] Inkey driver
* [ ] Hardware joystick
* [ ] File I/O
* [ ] Interrupts
* [ ] RS232

## Compilation

    zcc +s1mp3 file.c

This command will create a .bin image suitable for use with s1mulator.

## Running compiled code

   ./s1mulator -b a.bin,0 -e 0x600

## Notes about the port

The port has been derived from a version of s1sdk that was found on archive.org. It has not been tested on real hardware.


### Technical notes

* The libraries maintain an internal screen buffer that needs to be blitted to the LCD using `LCD_UpdateScreen()`.

## Links

* [S1 Emulator](http://github.com/suborb/s1mulator)
* [S1 LoadRAM](http://github.com/suborb/s1mp3-loadram) - used for loading binaries onto real hardware
