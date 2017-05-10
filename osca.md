#  Old School Computer Architecture

{{:platform:osca.jpg|}}

## Building applications


### Command Line

    zcc  +osca -lm -lndos -o program.exe program.c
-- or --
    zcc  +osca -lm -lflosdos -o program.exe program.c
-- or --
    zcc  +osca -subtype=ansi -lm -lflosdos -o program.exe program.c

The resulting 'a.bin' program can then be renamed to 'application.exe'.
Optionally the code can be relocated as specified in the more recent FLOS version by using the '-zorg' parameter.

## Extra Options

The OSCA binary format permits to specify a different memory position and to do a memory bank switch before loading.

###  C source level

    #pragma output osca_bank=(0..14)

Set the memory bank for locations > 32768 before loading program

    #pragma output osca_stack=`<value>` 
Put the stack in a differen place, i.e. 32767

###  compiler command line

 1. zorg=`<location>`

Permits to specify the program position

