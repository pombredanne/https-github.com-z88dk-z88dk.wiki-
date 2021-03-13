
![](images/platform/sam.jpg)


# Quick start

    zcc +sam -lm application.c

-- or --

    zcc +sam -clib=ansi -lm application.c

# Emulator notes

Use the [SimCoupé](http://www.simcoupe.org/) emulator or the [latest development builds](https://github.com/simonowen/simcoupe), press F4 and you can import any binary in the file system to any given BASIC address of the SAM Coupé.  For example, use 32768 as the import address and when returning to BASIC you can just CALL 32768 to run the code.

The SAM Coupé port has also been tested on the [SAMEMU](http://www.inf.upol.cz/~keprta/sam/samemu/welcome.html) emulator.
This emulator can load a file in ram (at address 32768) by pressing F1:

* compile the program (zcc +sam hello.c  --or--  zcc +sam -clib=ansi hello.c  )
* copy the compiled output to a file called "SAMFILE" (copy a.bin samfile -or- copy a.bin samfile )
* run the emulator, press F1 (apparently nothing happens) and type "call 32768".

# Real Hardware

You can use the *dskman* utility to save your files to a dsk-image which can be written out to a DD floppy and read by a real SAM Coupé.  

# Creating autoboot disk images

[pyz80 - a Z80 cross assembler](https://github.com/simonowen/pyz80)

org &8000

dump $

autoexec...
