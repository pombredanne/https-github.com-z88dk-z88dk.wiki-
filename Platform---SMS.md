![](images/platform/sms.jpg)

## Hardware specs

* Z80 @ 3.58Mhz
* 8k RAM
* 16k VRAM (VDP like)
* 8k Bios ROM
* SN76489 PSG

## Classic library support (`+sms`)

* [ ] Native console output
* [ ] Native console input
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
* [x] PSG sound
* [ ] One bit sound
* [ ] Inkey driver
* [x] Hardware joystick
* [ ] File I/O
* [x] Interrupts
* [ ] RS232


## Quick Start

    zcc +sms -create-app -o program.bin program.c

    zcc +sms -create-app -lgamegear -o program.bin program.c


The resulting file 'program.sms' can be loaded into an SMS emulator.

## Library support

The z88dk classic library comprises of a fusion of the [TMS99x8](Classic-TMS9918) with a donated custom library. This fusion allows the creation of either highly portable applications (using conio and the rest of the z88dk libraries) or applications leveraging the features of the SMS hardware.

## Generic console

The generic console currently only supports the 3 modes in common with the [TMS99x8](Classic-TMS9918) chip. Remember to add the `--generic-console` option to enable it at compile time to switch screen mode before printing - the classic +sms target starts up in mode 4.

## Gamegear support

A Gamegear will load .sms file and use hardware scaling to shrink the 256x192 TMS99x8 display to the 160x144 Gamegear LCD. This can make graphics a little fuzzy to say the least. Another key difference is that the Gamegear, unlike the SMS, will not automatically set the palette used by the TMS99x8 screen modes, however z88dk will set the appropriate palette entries following setting the mode. Thus, if you have a custom palette, remember to set it after switching screen mode. As an additional note, the palette for TMS99x8 modes on the Gamegear starts at index 16, rather than index 0 on the SMS.

z88dk can also produce .gg files when the option `-subtype=gamegear` is supplied. When a Gamegear loads a .gg file, hardware scaling isn't enabled and the only visible area of the screen is 160x144 which is centred within the 256x192 display. The z88dk generic console takes this into account and adjusts its size and location to match the visible area. In Gamegear mode, the palette hardware is much improved and the default palette used by z88dk more closely resembles the palette on other TMS99x8 machines.

The following function `load_palette_gamegear()` can be used in Gamegear mode to set that palette using 12 bit BGR values.


## Emulators

* [Emulicious](https://emulicious.net)
* MAME/MESS
* [MEDNAFEN](https://mednafen.github.io/)


### Example usage

    mednafen -force_module sms program.sms
    mednafen -force_module gg  program.sms
    mame sms1 program.sms
    mame gamegear program.sms




## Links

["stevepro studios" - z88dk Programming related articles](http://steveproxna.blogspot.it/search/label/z88dk)

[King Kong game](http://hirudov.com/sega/KingKongSMS.php)

[Thread about using sprites on SMS and z88dk](http://www.mastersystem-france.com/t1686p30-programmation-master-system-en-assembleur-variante-en-c) (French)

[Haroldo-OK website, some game for the SMS is written with z88dk](http://www.haroldo-ok.com/)

[SMSBomberman preview (YouTube video)](https://www.youtube.com/watch?v=akYolXhhL1Q)

