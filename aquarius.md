======= Mattel Aquarius hints & tools =======




# Quick start

    zcc +aquarius -lm -create-app -o adventure adv_a.c
-or-
    zcc +aquarius -subtype=ansi -lm -create-app -o adventure adv_a.c

The binary converter (appmake) will create two files: _adventure.caq and adventure.caq


The first one is the BASIC loader, and the second one is the binary block.
They should be loaded in sequence; type **CLOAD**, play the first file, then type **RUN** and play the second one.


# Emulator notes

Tested under the "Virtual Aquarius" emulator, by James the Animal Tamer.

	- run the emulator				aquarius
	- from within the emulator, load the loader:	cload
	- press ENTER twice, then play the "_adventure.caq" cassette file (loader)
	- at the "OK" prompt, type RUN <ENTER>
	- press ENTER twice, then play the "adventure.caq" cassette file
	- wait... then have fun !

# Links

[Yet another Death Star port](http://atariage.com/forums/topic/173559-intellivision-homebrew-istar-wip/)

[Sprite demo for the Mattel Aquarius](http://www.atariage.com/forums/topic/173909-aquarius-sprite-demo-complied-using-the-z88dk-devkit/)

[HW development idea: 320x200-bitmap-graphics-hack !](http://atariage.com/forums/topic/233221-aquarius-320x200-bitmap-graphics-hack/)

