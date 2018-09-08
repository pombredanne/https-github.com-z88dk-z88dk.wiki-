VTech Laser 350/500/700.

# Hardware summary

* Z80 @3.694700 MHz
* 32k ROM
* 16k banking model in all 4 segments
* Banked graphics memory (custom ASIC)
* 16/64/128k memory available

# Compilation

    zcc +vz700 world.c -create-app -Cz--audio

Will create a file that will load on real hardware.

    zcc +vz700 world.c -create-app -Cz--audio -Cz--fast

Will create a file that will load on the Mame emulator (Mame reads/writes files with a different speed to the real hardware)

To load and run the file enter:

    cload
    run

# Emulator

At present Mame is the only emulator that supports the VZ70 (tested with Mame 0.194)

# Features

## Generic console

The generic console is enabled by default on this port. The following screen modes are supported:

* Mode 0 - 40x24, 16 colours
* Mode 1 - 80x24, 2 colours

Both modes support chunky graphics.

To select the colours in mode 1, you can use `textbackground()/textcolor()` to select background/foreground colours. They will only take effect the next time the screen is cleared.

# Limitations

* The generated program will not work on the unexpanded (16k) VZ350
* Sounds, joysticks and hires graphics are not yet supported
* Scanning the keyboard temporarily disables interrupts
* Scanning the keyboard reportedly causes problems on real hardware

# Reference

* 40 column screen layout: http://atariage.com/forums/topic/187667-any-info-on-video-technology-laser-500-computer/page-6 (80 column is monochrome so attribute bytes presumably become extra characters)
* Rom detection: http://atariage.com/forums/topic/187667-any-info-on-video-technology-laser-500-computer/page-8 at 0x66c2 in ROM
* Wikipedia: https://es.wikipedia.org/wiki/VTech_Laser_700
* Paging information: http://www.razzmoket.esy.es/memoire.htm
