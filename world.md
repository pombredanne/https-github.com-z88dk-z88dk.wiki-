# Hello World example

Note the character code **12** used to clear the screen.


`<file>`
/*

 *	Hello World
 */

#include `<stdio.h>`

main()
{
	printf("%cHello world!\n",12);
}
`</file>`


## Running on the ZX Spectrum

Compile it with the following command:

    zcc +zx -lndos -create-app -o hello hello.c

It will make a file named "hello.tap".

When run the progam will produce the following output:

{{examples:snippets:hello.gif|}}



## Running on the ZX 81 in FAST mode

Compile it with the following command:

    zcc +zx81 -create-app -o hello hello.c

It will produce a file named "hello.p", with the following output:

{{examples:snippets:hello81.gif|}}



## Running on the MSX

Compile it with the following command:

    zcc +msx -create-app hello.c

It will produce a file named "a.msx", which can be transferred onto a disk image and run:

{{examples:snippets:hello_msx.gif|}}



