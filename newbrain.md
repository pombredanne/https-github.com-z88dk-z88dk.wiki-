 ======  Grundy Newbrain  Platform ======

## Introduction

The Grundy Newbrain platform is still supported quite at a base level, but it already features a powerful appmake tool.


## Building applications

The binary code can be relocated or left at its default position at 10000 (decimal).




### Command Line

    zcc  +newbrain -lm -lndos -create-app program.c

-or-

    zcc  +newbrain -lm -lnbdrv -create-app program.c


The above examples will build a BASIC loader, a binary block and a "directory file", suitable for the tape support of the Newbrain emulator by Chris Despinidis.
In the second case the file library is linked in place of the foo fcntl one.






## Advanced options



    zcc  +newbrain -lm -startup=2 -create-app program.c

The "startup=2" option enables an irq extender, still under development.  This option won't work on expanded models.

----


    zcc  +newbrain -lm -zorg=20000 -create-app program.c

The "-zorg=`<location>`" option locates the code at the desired position; it has effect also on the "appmake" tool, which will adapt the loader for the new location.

----


    zcc  +newbrain -lm -Cz-o -Cz-myname -create-app program.c

It is an advanced way to pass parameters to the appmake tool; in this case we've specified a different file name.
The available options are:

`<file>`

 1. h   --help            (bool)    Display this help
 2. b   --binfile         (string)  Linked binary file
 3. c   --crt0file        (string)  crt0 file used in linking
 4. o   --output          (string)  Name of output file
 5. a   --ascii           (bool)    Generate a BASIC loader in plain text
       --blockname       (string)  Name of the binary block in tape
       --org             (integer) Origin of the binary

`</file>`


## Tips and tricks

### Disk expansion

To select the tape device in disk-enabled models, you need to change the default I/O device before LOADing the program:

    POKE 19,1

Then you can use the LOAD command as usual.


### Using CP/M

The [CP/M](platform/cpm) target environment can be extended with Newbrain specific functions simply linking the "nbcpm" library.

Refer to the [library documentation](library/newbrain) for details.

