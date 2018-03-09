#  Enterprise 64/128 Platform

![](images/platform/enterprise.gif)


## Building applications

The two currently supported binary formats are both originated at address 100h.

### Command Line

This command generates a file called 'program.app', which can be loaded from BASIC with the 'load' or 'run' commands (e.g. run "a.app"):

    zcc  +enterprise -lm -create-app -oprogram program.c


This command generates a file called 'program.com', with no special headers:

    zcc  +enterprise -subtype=com -lm -oprogram.com program.c

In this latter example a protection for MS/DOS is automatically inserted, though it can be excluded adding in your source program the "#pragma no-protectmsdos" directive.

Optional libraries are provided for a partial support of the 'standard' monochrome graphics, use the "-gfxep" parameter for a 336×243 resolution and "-lgfxephr" for 672×243.
Some implementations are slow and incomplete, but being totally connected via the EXOS the libraries should bear video mode tweakings.


## Notes on the emulators

The z88dk port has been tested on two emulators, ep32 and ep128emu.

To load a .app file with ep128, follow the instructions on http://ep128.hu/Ep_Emulator_eng.htm regarding enabling EPFileIO.rom Once configured enter `run "file:"` from BASIC and select your compiled application.


The way the text mode is emulated seems to be different in colour mode, so a monochrome 40x25x2 mode is active by default.
In case no screen is visible with ep128emu on old PCs, try passing the '-no-opengl' parameter at startup.


## External Links

[Technical informations](http://ep.homeserver.hu/Dokumentacio/Konyvek/EXOS_2.1_technikal_information/index.html)
