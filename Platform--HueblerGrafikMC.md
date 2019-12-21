# Hardware summary

* U880 @ 1.5Mhz
* 48k contiguous RAM
* B&W 256x256 pixel display

## Classic library support (`+hemc`)

* [x] Native console output
* [x] Native console input
* [ ] ANSI vt100 engine
* [x] Generic console
    * [x] Redefinable font 
    * [x] UDG support
* [ ] Lores graphics
* [x] Hires graphics
* [ ] PSG sound
* [ ] One bit sound
* [ ] Inkey driver
* [ ] Hardware joystick
* [ ] File I/O
* [x] Interrupts
* [ ] RS232

This is a relatively bare-bones machine. Two serial ports are available but as of yet they aren't supported by z88dk.

# Compilation

    zcc +hgmc program.c -create-app

This will create a KCC file that can be loaded with the jkcemu emulator.

Once loaded, enter the following command to run:

    GO 0000

# Graphics

High-resolution graphics are available at 256x256 pixels.


# Links

* [Technical information](https://hc-ddr.hucki.net/wiki/doku.php/homecomputer/huebler#hueblerevert-mc)
* [JKCEmu - Emulator](http://www.jens-mueller.org/jkcemu/index.html)
