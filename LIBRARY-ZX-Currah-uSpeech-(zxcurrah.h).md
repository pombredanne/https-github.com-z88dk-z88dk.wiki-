|            |                              |
| ---------- | ---------------------------- |
| Header     | [{z88dk}/include/zxcurrah.h](https://raw.githubusercontent.com/z88dk/z88dk/master/include/zxcurrah.h)    |
| Source     | [{z88dk}/libsrc/spectrum/currah_detect.asm](https://github.com/z88dk/z88dk/tree/master/libsrc/spectrum/currah_detect.asm),
[{z88dk}/libsrc/spectrum/currah_speech.asm](https://github.com/z88dk/z88dk/tree/master/libsrc/spectrum/currah_speech.asm),
[{z88dk}/libsrc/spectrum/currah_direct.asm](https://github.com/z88dk/z88dk/tree/master/libsrc/spectrum/currah_direct.asm)                   |
| Include    | `#include <zxcurrah.h>`      |
| Linking    | n/a                          |
| Compile    | n/a                          |
| Comments   |                              |

The Currah USpeech is a voice synthetizer for the ZX Spectrum.
The current API permits both a low level and an high level control, plus an hardware detection tool.


# Detection


## int currah_detect()

Returns TRUE if the interface is present



# Control


## void currah_speech(char *text)

Talk using the high level BASIC interface


## void void currah_direct(char *allophones)

Talk using the allophone codes
(see header file)

# Sample Code

```

/*
	ZX Spectrum and the Currah uSpeech lib demo

*/
#include `<spectrum.h>`

#include `<zxcurrah.h>`
#include `<stdio.h>`

#include `<stdlib.h>`

/* 'Hello" word for direct mode */
char hello[] = {PH_H, PH_E | PH_PITCH, PH_LL, PH_O, PH___, PH_END};

int main()
{
	if ( !currah_detect() )  
	{
		printf ("CURRAH uSpeech is present\n");
	}
	else
	{
	printf ("Hello (from the direct engine)\n");
	currah_direct (hello);
	sleep (1);
	printf ("\nHello (internal conversion functions)\n");
	currah_speech ("hE(ll)o");
	sleep (1);

	printf ("\n\nI am a ZX Spectrum talking\n");
	currah_speech ("aY em a zed eks spEctrum tokin");

	}

	printf ("\n\n\n(Program end).\n");
}

```
