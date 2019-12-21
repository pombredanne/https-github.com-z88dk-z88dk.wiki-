# Hardware summary

* U880 @ 1.5Mhz
* B&W 64x16 display

## Classic library support (`+hemc`)

* [x] Native console output
* [x] Native console input
* [ ] ANSI vt100 engine
* [x] Generic console
    * [ ] Redefinable font 
    * [ ] UDG support
* [x] Lores graphics
* [ ] Hires graphics
* [ ] PSG sound
* [ ] One bit sound
* [ ] Inkey driver
* [ ] Hardware joystick
* [ ] File I/O
* [ ] Interrupts
* [ ] RS232

This is a relatively bare-bones machine.

# Compilation

    zcc +kramermc program.c -create-app

This will create a KCC file that can be loaded with the jkcemu emulator.

# Graphics

A very lores 64x16 graphics mode is supported. 

# Links

* [Technical information](https://hc-ddr.hucki.net/wiki/doku.php/homecomputer/kramermc)
* [JKCEmu - Emulator](http://www.jens-mueller.org/jkcemu/index.html)
