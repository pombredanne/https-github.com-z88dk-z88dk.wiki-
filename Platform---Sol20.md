# Sol20 Hardware summary

* 8080 @ ~2Mhz
* S-100 bus
* Upto 48k contiguous RAM
* VDM1 graphics board

## Classic library support (`+sol20`)

* [ ] Native console output
* [ ] Native console input
* [ ] ANSI vt100 engine
* [x] Generic console
    * [ ] Redefinable font 
    * [ ] UDG support
    * [ ] Paper colour
    * [ ] Ink colour
    * [x] Inverse attribute
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


# Compilation

    zcc +sol20 program.c -create-app

An .ent file is created suitable for loading into an emulator. Alternatively an audio file for use on real hardware can be created with:

    zcc +sol20 -subtype=wav program.c -create-app

# Emulation usage

Using the Solace emulator, load the .ENT file using File->Load Program. Then enter the following
to start the program:

```
EX 0
```


# Links

* [Solace Emulator](http://www.sol20.org/solace.html)
