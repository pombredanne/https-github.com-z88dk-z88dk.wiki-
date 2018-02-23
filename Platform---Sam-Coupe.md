
{{platform:sam.jpg|}}

You can use the *dskman* utility to save your files to a dsk-image which can be written out to a DD floppy and read by a real SAM Coupé.  

# Quick start

    zcc +sam -lm application.c

-- or --

    zcc +sam -clib=ansi -lm application.c

# Emulator notes

Use the [SimCoupè](http://sourceforge.net/projects/simcoupe/) emulator, press F3 and you can now import any binary in the file system to any given address of the SAM Coupé.  When returning to BASIC you can **CALL** [address] as usual.

The SAM Coupe port has also been tested on the [SAMEMU](http://www.inf.upol.cz/~keprta/sam/samemu/welcome.html) emulator.
This emulator can load a file in ram (at address 32768) by pressing F1:

```
- compile the program (zcc +sam hello.c  --or--  zcc +sam -clib=ansi hello.c  )
- copy the compiled output to a file called "SAMFILE" (copy a.bin samfile -or- copy a.bin samfile )
- run the emulator, press F1 (apparently nothing happens) and type "call 32768".
```
