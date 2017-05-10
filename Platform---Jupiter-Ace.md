======= The Jupiter ACE =======

The Jupiter ACE has a built-in FORTH interpreter in ROM (no BASIC, here!)
There is very little software for it;  maybe the Z88DK will help !

{{platform:ace.jpg|}}



# Quick start


The J.ACE port comes with a newer version of appmake tool capable of creating 'tap' and 'wav' files.
See the "Older emulator hints" chapter to use z88dk with the VACE or ACE32 emulators.

This newer release has been developed with the aid of the EightyOne emulator and tested on the real computer.
To compile a program (with 'a.bin' as the default program name), type:


    zcc +ace -create-app program.c

The program can then be loaded with the following command:

    0 0 bload a.bin



To insert the ANSI VT emulation, and change the default program name, type:

    zcc +ace -clib=ansi -create-app -Cz--audio -o program program.c



The '--audio' parameter adds the creation of a 'wav' file in addition to the 'tap' format.   Specifying also the '--fast' flag the audio file will be create in a sort of 'turbo' mode.

Setting '-subtype=rom" will provide a very primitive skeleton for a ROM replacement program, be aware that most of the I/O z88dk functions simply won't work, you need to provide your own.


# Appmake extras

The appmake tool can be run in "dumb" mode to generate the corresponding audio track of some external program.

    C:\>appmake +ace --dumb --audio -b program.tap
    
The optional "--fast" flag will produce a non-standard audio track which, even if faster, should be still loadable by the real computer.



# FORTH command syntax to LOAD/SAVE bytes

**s l bsave `<name>` saves l bytes from the memory starting at address s as `<name>` 

**s l bload `<name>` loades l bytes to the memory starting at address s as `<name>`. If s or l is zero will their value be taken from the file. 

**addr call** will call Z80 machine code at addr, should be terminated with a jp (iy) Z80 instruction. 


The newer TAP format features an autorun mode, so che 'call' is given automatically.


# Older emulators hints

The classic version (bin2byt) is now available only as a separate tool in the {z88dk}/support/Ace directory.


#### Using the "VACE" emulator

 1.  compile the program:

    * zcc +aceansi -lm -o world world.c
 2.  convert it:

    *bin2byt world world.byt
 3.  run the emulator

    * vace
 4.  from within the emulator, load the program:

    * 16384 40000 bload world
 5.  run the thing:

    * 16384 call


#### Using ACE32 on WinNT/Win2000

Get the file:
http://www.jupiter-ace.co.uk/ace32.html

 1.  compile the program:

    * zcc +ace program.c
 2.  delete the old TAP file

    * del prog.tap
 3.  convert it:

    * acetap a.bin prog.tap
 4.  run the emulator

    *ace32 prog.tap
 5.  from within the emulator, load the program:

    *16384 40000 bload
 6.  run the thing:

    *16384 call


