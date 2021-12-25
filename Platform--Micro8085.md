## Hardware summary

Home brew hardware with:

* Intel 8085
* 32k ROM/32k RAM
* Intel 8155 IO & timer
* Intel 8251 UART
* Support for SPI bus, serial 32k e2prom, ADC, DAC

Target has no built in monitor software or other 'OS' for program download. User program location is straight into ROM.

## Classic library support (`+micro8085`)

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

# Compilation

    zcc +micro8085 program.c -o output.bin -create-app

A .ihx file is produced suitable for burning to ROM.

## Links

* https://hackaday.io/project/176653-micro8085
