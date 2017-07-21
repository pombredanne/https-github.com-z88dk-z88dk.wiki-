# Philips Videopac C7420

The Philips C7420 was an expansion for the Philips Videopac gaming console.
It consists in a second CPU (Z80), extra memory and a Microsoft BASIC interpreter.

# Quick start

    zcc +c7420 -create-app program.c

This will genetrate a file, **_a.bas**, (a binary tape image) suitable for the O2EM emulator or for the [BASIC.JAR](http://www.ifs.tuwien.ac.at/dp/hc_audio_migration/basic.jar) tool (a Java VM is required in this latter case).  The file name can be varied with the '-o' option, and can be loaded with the command **CLOAD "_a"** (or similar).

    zcc +c7420-create-app -zorg=<location> program.c

This is a further experimental mode, meant to create two cassette block files, a BASIC loader (i.e. _a.bas) and a block created in "CSAVEM" mode (i.e. a.bas); it is untested and probably not fully working.

# Emulator hints

The o2em emulator needs to be invoked with the proper parameter to enable the BASIC extension cartridge:

    o2em -config=c7420


To boot the cartridge, at the splash startup screen, press '0',  then use the "CLOAD" command as already explained.

The programs must be put in the "/basic" subfolder.

# Links

[The unofficial C7420 emulator at 'guttenbrunner.com'](http://guttenbrunner.com/videopac/)
 ... hidden around there [O2EM 1.20 Beta 5](http://guttenbrunner.com/videopac/o2em120B5win.zip)

[The C7420 migration tool](http://www.ifs.tuwien.ac.at/dp/hc_audio_migration) by the Vienna University of Technology, also containing a pre-released [O2EM 1.21](http://ifs.tuwien.ac.at/dp/o2em/o2em-1.21.zip)




