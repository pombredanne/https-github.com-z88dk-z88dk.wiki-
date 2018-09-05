# Hardware summary

* Z80 @3.694700 MHz
* 32k ROM
* 16k banking model in all 4 segments
* Banked graphics memory (custom ASIC)
* 16/64/128k memory available

_This machine is currently being brought up_

# Compilation

    zcc +vz700 world.c -create-app -Cz--audio

Will create a file that will load on real hardware.

    zcc +vz700 world.c -create-app -Cz--audio -Ca--fast

Will create a file that will load on the Mame emulator.

To load and run the file enter:

    cload
    run

# Features

# Reference
