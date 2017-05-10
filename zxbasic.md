# Calling BASIC

This example shows the capabilities of the functions to interface to the ZX Spectrum BASIC.
It is possible to pass parameters and to call "subroutines".


```

/*
	Interfacing to the ZX Spectrum BASIC
	demonstration on:
	 - diagnostics
	 - parameter passing
	 - code execution
	 
	Try adding BASIC commands in line 100 and 150, like
	100 LET a$="Test": STOP
	
	Then, after run, try to PRINT b$

*/

#include `<spectrum.h>`

#include `<zxcurrah.h>`
#include `<zxinterface1.h>`

#include `<stdio.h>`

char	value[100];

int main()
{
	if ( if1_installed() )  printf ("Interface 1 is active\n");
	if ( if1_from_mdv() )  printf ("Program loaded from microdrive\n");

	if ( currah_detect() )  printf ("CURRAH uSpeech is present\n");

	if ( zx_issue3() )
	{
		printf ("This is a Spectrum issue 3 or more\n");
	}
	else
	{
		printf ("This Spectrum is issue 1 or 2 !\n");
	}

	if ( zx_128mode() )
	{
		printf ("This Spectrum is working in 128K mode\n");
	}
	else
	{
		printf ("This Spectrum is working in non-128K mode\n");
	}

	printf ("Basic length: %u\n",zx_basic_length() );
	printf ("Variables area length: %u\n",zx_var_length() );
	printf ("Basic exec line 100 result: %u\n",zx_goto(100) );
	printf ("Basic exec line 150 result: %u\n",zx_goto(150) );

	zx_getstr ('a',value);
	printf ("Got string value in 'a' :  %s\n",value);

	zx_setint ("n",100);
	zx_setint ("num",1234);

	printf ("Got numeric value in 'num' :  %u\n", zx_getint ("num") );
	printf ("Got numeric value in 'n' :  %u\n", zx_getint ("n") );
	
	zx_setstr ('b',"This is the b$ string, which nobody can deny...");
	
	printf ("\n\nProgram end\n");

}

```


## Running on the ZX Spectrum

Compile it with the following command:

    zcc +zx -lndos -create-app -o zxbasic zxbasic.c

It will make a file named "zxbasic.tap".


Before running it you need to modify the BASIC portion:

    MERGE ""

then insert a couple of BASIC program lines:

    100 LET a$="Test": STOP
    150 BORDER 5: STOP

Now you can restart the loader...

    RUN

...you should get an output like this:

{{examples:snippets:zxspectrum:zxbasic.gif|}}
