# Hardware summary

* Z80 @ 4 Mhz
* 64k RAM, 32k ROM 
* MC6847
* AY-3-8192
* Optional TMS9928A

# Compilation

    zcc +spc1000 world.c -create-app

The resulting .rom file can be loaded into espc1000.exe emulator:

1. Tape->Play select .spc file
2. Type `LOAD`
3. Tape->Play Button

The file will load and autorun.

# Screen modes and graphics

The console subsystem for the PC6001 is completely decoupled from the ROM, this allows z88dk to transparently support multiple screen modes.

Screen modes can be changed using the following code:

    #include <sys/ioctl.h>

    int mode = 1;
    console_ioctl(IOCTL_GENCON_SET_MODE, &mode);

The following modes are supported:

* Mode 0 - 32x16 text, supports coloured block graphics with a resolution of 64x48
* Mode 1 - 256x192, monochrome
* Mode 2 - 128x192, colour

With Mode 1 and Mode 2 you will need to define a [font](Classic-GenericConsole#defining-a-custom-font) that can be used to print on console. Mode 1 and 2 support UDGs of course.

To change the pixel colour you can call `textcolor()` with the desired colour.

Of course, supported multiple screen modes does have an overhead, as a result if you don't use either mode 1 or mode2, then you can exclude their code from being included using the following pragmas: `-pragma-define:CLIB_DISABLE_MODE0=1` and `-pragma-define:CLIB_DISABLE_MODE1=1`
	
The optional VDP is also supported - compile with `-pragma-define:CRT_ENABLE_VDP=1`, select mode 10, 11 or 12 and switch monitors.

# Screenshots


# Limitations


# Links

* [Technical Docs](https://github.com/meesokim/spc1000/tree/master/docs)
