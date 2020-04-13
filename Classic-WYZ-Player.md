# Introduction

WYZPlayer is a cross-platform music and sound effects player for targets containing an AY-891x.

## Machine support

The following targets support interrupt based playing:

- MSX
- ZX Spectrum
- TS2068
- ZX Spectrum Next
- Nichibutsu My Vision
- Bandaivision SV8000 (untested)

The following are busy loop only:

- Multi 8 
- PC6001

## Playing music

The API to the tracker library is defined in `<psg/wyz.h>` and consists of 4 functions:

```
extern void __LIB__ ay_wyz_init(wyz_song *song) __z88dk_fastcall;
extern void __LIB__ ay_wyz_play(void);  // Called on interrupt, trashes main register + ix,iy
extern void __LIB__ ay_wyz_start(int song) __z88dk_fastcall; // Setup to play song N
extern void __LIB__ ay_wyz_stop(void);  // Stop playing
```

The main function is `ay_wyz_play()` which should be called on the interrupt. 

## Converting WYZTracker output to usable files

WYZTracker outputs two files: `tune.mus` and `tune.mus.asm`. Both of these files should be copied into your project.

`tune.mus.asm` needs to be modified to turn it into a format that can be used by z88dk. So open the file using an editor.

At the top of the file paste the following:

```
        SECTION rodata_user

        PUBLIC  _mysong


_mysong:
        defw    TABLA_SONIDOS
        defw    TABLA_PAUTAS
        defw    DATOS_NOTAS
        defw    SONG_TABLE


SONG_TABLE:
        defw    SONG_01

SONG_01:
        BINARY  "tune.mus"
```

You can change `_mysong` to any other label - make sure it starts with an underscore and change it both places! If you do so, don't forget to change the parameter name in the call to `ay_wyz_init()`

Assuming that the C file is `main.c`, then you can compile with:

    zcc +zx main.c tune.mus.asm -create-app

or

    zcc +msx -subtype=rom main.c tune.mus.asm -create-app

## Sound effects

Sound effects are controlled using the following functions:

```
extern void __LIB__ ay_wyz_effect_init(wyz_effect *effect);
extern void __LIB__ ay_wyz_start_effect(int channel, int effect_number); // Play an effect
extern void __LIB__ ay_wyz_stop_effect(void);  // Stop playing effect
```

The playing of effects is handled by the `wyz_play()` function introduced above.

# Example

An example showing both playing music and effects is available in the [examples/sound/wyz](https://github.com/z88dk/z88dk/tree/master/examples/sound/wyz) directory.


