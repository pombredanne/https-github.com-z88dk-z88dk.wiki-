![](images/platform/m5b.jpg)

AKA Gold Star Electronics FC-150 (Famicom-150).


# Machine Specifications

* CPU: Z80 @ 3.579 Mhz
* VDP: TMS9918, 16k VRAM
* PSG: SN76489
* RAM: 4k (*)
* ROM: 8k

# Compilation

    zcc  +m5 -create-app  program.c

This will create a .rom file that is suitable for loading into emulators.

## Integer BASIC cartridge extension (obsolete)

This configuration is not recommended - it's much easy to generate and load a .ROM image.

    zcc  +m5 -create-app -subtype=tape program.c

This will create both a 'CAS' and a WAV file, for emulators or the real hardware.
To load and run the program use the SORD BASIC 'TAPE' command.
To load a program with no BASIC cartridge is possible, the IPL ROM expects the tape to be played, no extra commands are necessary.

# Memory usage

By default, the z88dk compile will assume that only 4k of memory is present (as per the real hardware), however some emulators support RAM extensions. In this case you can move the stack pointer to a higher location and hence use more variables or heap space. To move the stack pointer add the option `-pragma-define:REGISTER_SP=65000`, or choose a value to your liking.

# Interrupts

The monitor ROM sets up im2 and places 4 vectors at address $7000 to handle interrupts raised by the CTC chip. The monitor rom uses vector 2 ($7006) to react the VDP vertical blank interrupt and also to read the keyboard. For the default M5 compile target this has been overridden and all keyboard handling is performed by the z88dk libraries.

# Hints on emulators

## Takeda Emulator

The ROM target was tested on the Takeda emulator.

The same emulator supports the WAV files only and features an accelerated load process.
Choose the file to be loaded in the 'tape' menu, then play the file.   No return message is provided when the loading phase is completed, but it is expected to last a couple of seconds:  keep an eye on the frame counter and the CPU percentage in the title bar and wait it to be reasonably stable to be sure the file was transferred, then go back to the 'tape' menu and close the file to permit the program to start.

## Mame

## Old hints for the emulator by Joseba Epalza.

zcc  +m5 -lm -zorg 29696 program.c

This will create a binary file.

To upload something on the emulator use the "inject utility" from the /support/generic folder.

	- run the emulator
	- press F3 to save a clean RAM image
	- exit from emulator (ESC)
	- rename dump.ram to dump.img
	- compile a program  (zcc +m5 hello.c)
	- "inject" the program into the RAM dump (inject a.bin dump.img 1024 dump.ram)
	- run the emulator
	- press F4 to load the modified RAM dump
	- type "call 29696" and enjoy !


