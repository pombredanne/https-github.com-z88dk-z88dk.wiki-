# Hardware summary

* U880 @ 2.4576Mhz
* 58k contiguous RAM
* B&W 64x24 display

## Classic library support (`+hemc`)

* [x] Native console output
* [x] Native console input
* [ ] ANSI vt100 engine
* [x] Generic console
    * [ ] Redefinable font 
    * [ ] UDG support
* [ ] Lores graphics
* [ ] Hires graphics
* [ ] PSG sound
* [ ] One bit sound
* [ ] Inkey driver
* [ ] Hardware joystick
* [ ] File I/O
* [x] Interrupts
* [ ] RS232

This is a relatively bare-bones machine. Two serial ports are available but as of yet they aren't supported by z88dk.

# Compilation

    zcc +hemc program.c -create-app

This will create a KCC file that can be loaded with the jkcemu emulator.

Once loaded, enter the following command to run:

    GO 0000

# Links

* [Technical information](https://hc-ddr.hucki.net/wiki/doku.php/homecomputer/huebler#hueblerevert-mc)
* [JKCEmu - Emulator](http://www.jens-mueller.org/jkcemu/index.html)
