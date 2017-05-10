# ANSI TEST example

This program will produce VT/ANSI compatible ESCAPE sequences to test the emulation engine.


`<file>`

#include "stdio.h"

main()
{
int x;

/* Set default text attributes (white on black) */
    printf ("%c[m",27);

/* Clear screen in non-ANSI mode*/
    printf ("%c",12);

/*
        A stupid CSI escape code test
        (normally no use uses CSI)

*/
    printf ("If you can read this, CSI is not working.\n");
    printf ("%c2J",155);
    printf ("If this is the first thing you can read, CSI is OK.\n");


/*
        Set Graphic Rendition test

*/
    printf ("%c[1mBold Text\n",27);
    printf ("%c[2mDim text\n",27);
    printf ("%c[4mUnderlined Text\n",27);
    printf ("%c[24mUn-underlined text\n",27);
    printf ("%c[7mReverse Text\n",27);
    printf ("%c[27mUn-reverse text\n",27);

    for (x=37; x>29; x--)
    {
    printf ("%c[%umFore text color %u.\n",27,x,x);
    }

    for (x=40; x<48; x++)
    {
    printf ("%c[%umBack text color %u.\n",27,x,x);
    }

/*
        Restore default text attributes

*/
    printf ("%c[m",27);

/*
        Cursor Position test
        "Draw" an X

*/

    for (x=0; x<11; x++)
    {
    printf ("%c[%u;%uH*\n",27,10+x,25+x);
    printf ("%c[%u;%uH*\n",27,20-x,25+x);
    }

}

`</file>`


## Running on the ZX Spectrum

Compile it with the following command:

    zcc +zx -clib=ansi -lndos -create-app -o ansitest ansitest.c

It will make a file named "ansitest.tap", producing the following output:


{{examples:snippets:ansitest_zx.gif|}}


## Running on the Sharp MZ 700

Compile it with the following command:

    zcc +mz -clib=ansi -create-app -o ansitest ansitest.c

It will make a file named "ansitest.m12", producing the following output:

{{examples:snippets:ansitest_mz.gif|}}


## Running on the MicroBEE

Compile it with the following command:

    zcc +bee -create-app -clib=ansi -o ansitest ansitest.c

It will make the file "ANSITEST.BEE".   The display will be intialized to 80x25 columns:

{{:examples:snippets:ansitest-bee.gif|}}





## Running on the Amstrad CPC

Compile it with the following command:

    zcc +cpc -clib=ansi -lndos -create-app -o ansitest ansitest.c

It will make a file named "ansitest.cpc" and a pure binary version (ansitest.bin).


Running in MODE 1: {{examples:snippets:ansitest_cpc_1.gif|ANSITEST running in MODE 1}}

To show all the text colors, the program can be run in video mode 0:

{{examples:snippets:ansitest_cpc_0.gif|ANSITEST running in MODE 0}}


## Running on the Luxor ABC 80

Compile it with the following command:

zcc +abc80 -clib=ansi -create-app -oansitest ansitest.c

It will make a file named "ansitest.bac", containing a BASIC loader with the machine code program stored in DATA lines.

{{examples:snippets:ansitest_abc80.gif|}}


## Running on the Philips VG-5000

Compile it with the following command:

zcc +vg5k -subtype=wav -clib=ansi -create-app ansitest.c

It will make a .K7 file and its related .WAV file.

To run it, just CLOAD it on your VG-5000 and run with "CALL 20480".


{{:examples:snippets:ansitest-vg5k.gif|}}


## Running on the Robotron Z9001 (or KC87)

Compile it with the following command:

zcc +z9001 -clib=ansi -create-app ansitest.c

It will make a .tap file.


{{:examples:snippets:ansitest-z9001.gif|}}


## Running on the Commodore 128

Compile it with the following command:

    zcc +c128 -clib=ansi ansitest.c

Convert it to a disk compatible format, and put it in a disk image with the c1541 tool (from the VICE emulator package):

    bin3000 a.bin z80mc
    c1541 loader.d64
    c1541 #8> write z80mc
    Writing file `Z80MC' as `Z80MC' to unit 8.
    c1541 #8> exit

Use the Z80RUN loader to run it in z80mode; the following output is generated:

{{examples:snippets:ansitest-c128.gif|}}



# ANSI TEST example (version for small displays)

This is a different version of the same example code for tiny, monochrome displays.
It is meant for the LCD based devices.

`<file>`

#include "stdio.h"

main()
{
int x;

/*
        A stupid CSI escape code test
        (normally no use uses CSI)

*/
    printf ("If you can read this, CSI is not working.\n");
    printf ("%c2J",155);
    printf ("If this is the first thing you can read, CSI is OK.\n");


/*
        Cursor Position test
        "Draw" an X

*/

    for (x=0; x<5; x++)
    {
    printf ("%c[%u;%uH*\n",27,1+x,21+x);
    printf ("%c[%u;%uH*\n",27,5-x,21+x);
    }


/*
        Set Graphic Rendition test

*/
    printf ("%c[1mBold Text\n",27);
    printf ("%c[2mDim text\n",27);
    printf ("%c[4mUnderlined Text\n",27);
    printf ("%c[24mUn-underlined text\n",27);
    printf ("%c[7mReverse Text\n",27);
    printf ("%c[27mUn-reverse text\n",27);

/*
        Restore default text attributes

*/
    printf ("%c[m",27);

}

`</file>`



## Running on the TI82

Compile it with the following command:

    zcc +ti82 -clib=ansi -create-app -oansitest ansitest.c

It will create a file named "ansitest.82p"; the following picture shows its output when run in the "CRASH" shell:

{{examples:snippets:ansitest_ti82.gif|}}


## Running on the TI83

Compile it with the following command:

    zcc +ti83 -clib=ansi -create-app -oansitest ansitest.c

It will create a file named "ansitest.83p"; the following picture shows its output when run in the "ION" shell:

{{examples:snippets:ansitest_ti83.jpg|}}


## Running on the TI83 Plus

Compile it with the following command:

    zcc +ti8x -clib=ansi -create-app -oansitest ansitest.c

It will create a file named "ansitest.8xp"; the following picture shows its output when run in the "ION" shell:

{{examples:snippets:ansitest_ti8x.jpg|}}


## Running on the TI85

Compile it with the following command:

    zcc +ti85 -clib=ansi -create-app -oansitest ansitest.c

It will create a file named "ansitest.85s"; the following picture shows its output when run in the "RIGEL" shell:

{{examples:snippets:ansitest_ti85.jpg|}}


## Running on the TI86

Compile it with the following command:

    zcc +ti86 -clib=ansi -create-app -oansitest -startup=10 ansitest.c

It will create a file named "ansitest.86p"; the following picture shows its output when run with the command "asm(ansitest)":

{{examples:snippets:ansitest_ti86.jpg|}}



## Running on the Sharp OZ 7XX organizers

Compile it and convert the binary file with the following command:

    zcc +oz -clib=ansi -oansitest.bin ansitest.c
    makewzd ansitest

It will create a file named "ansitest.wzd":

{{examples:snippets:ansitest_oz.gif|}}
