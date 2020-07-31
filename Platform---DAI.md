Hardware summary

* 8080A  @ ~2Mhz
* 8,12,32,36,48k RAM
* 24k ROM
* Option AM9511 maths coprocessor

## Classic library support (`+dai`)

* [x] Native console output
* [x] Native console input
* [ ] ANSI vt100 engine
* [ ] Generic console
    * [ ] Redefinable font 
    * [ ] UDG support
    * [ ] Paper colour
    * [ ] Ink colour
    * [ ] Inverse attribute
    * [ ] Bold attribute
    * [ ] Underline attribute
* [ ] Lores graphics
* [ ] Hires graphics
* [ ] PSG sound
* [ ] One bit sound
* [ ] Inkey driver
* [ ] Hardware joystick
* [ ] File I/O
* [ ] Interrupts
* [ ] RS232

_This machine is currently being worked on, hopefully more features will be forthcoming._

# Compilation

    zcc +dai program.c -create-app

A .wav file is produced suitable for loading as a tape file into, for example Mame.

To load type:

    UT

To enter the monitor, then type:

   R

When loaded, the start + end addresses will be displayed on screen. To start the program type:

    G0800

# Links

* https://electrickery.hosting.philpem.me.uk/comp/dai/index.html
