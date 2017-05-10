# ZX Spectrum Currah uSpeech API (zxcurrah.h)

| | |
|----|---|
| Header     | [{z88dk}/include/zxcurrah.h](https///raw.githubusercontent.com/z88dk/z88dk/master/include/zxcurrah.h)    |
| Source     | [{z88dk}/libsrc/spectrum/currah_detect.asm](https///github.com/z88dk/z88dk/tree/master/libsrc/spectrum/currah_detect.asm),  [{z88dk}/libsrc/spectrum/currah_speech.asm](https///github.com/z88dk/z88dk/tree/master/libsrc/spectrum/currah_speech.asm),  [{z88dk}/libsrc/spectrum/currah_direct.asm](https///github.com/z88dk/z88dk/tree/master/libsrc/spectrum/currah_direct.asm)                   |
| Include    | `#include <zxcurrah.h>`             |
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

