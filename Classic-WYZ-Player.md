# Introduction

WYZPlayer is a cross-platform music and sound effects player for targets containing an AY-891x.

## Machine support

The following targets support interrupt based playing:

- MSX
- ZX Spectrum
- Nichibutsu My Vision
- Bandaivision SV8000 (untested)

The following are busy loop only:

- Multi 8 
- PC6001

## API

The API to the tracker library is defined in `<psg/wyz.h>` and consists of 4 functions:

```
extern void __LIB__ ay_wyz_init(wyz_song *song) __z88dk_fastcall;
extern void __LIB__ ay_wyz_play(void);  // Called on interrupt, trashes main register + ix,iy
extern void __LIB__ ay_wyz_start(int song) __z88dk_fastcall; // Setup to play song N
extern void __LIB__ ay_wyz_stop(void);  // Stop playing
```

The main function is `ay_wyz_play()` which should be called on the interrupt. The following example demonstrates how to setup the library and an interrupt routine to play music:

```
#include <stdio.h>
#include <intrinsic.h>
#include <interrupt.h>
#ifdef __SPECTRUM__
#include <spectrum.h>
#endif
#ifdef __MSX__
#include <msx.h>
#endif
#include <psg/wyz.h>
#include <stdlib.h>


#if __PC6001__ | __MULTI8__
#define NO_INTERRUPT 1
#endif

extern wyz_song mysong;

void playmusic(void) {
   M_PRESERVE_MAIN;
   M_PRESERVE_INDEX;
   ay_wyz_play();
   M_RESTORE_INDEX;
   M_RESTORE_MAIN;
}


void setup_int() {
#ifndef NO_INTERRUPT
#if __SPECTRUM__
   zx_im2_init(0xd300, 0xd4);
#else
   im1_init();
#endif
  add_raster_int(playmusic);
#endif
}


void main()
{
   //printf("%cWYZ Tracker example\n",12);

   // Load the tracker file
   ay_wyz_init(&mysong);
   // Play song 1 within the  file
   ay_wyz_start(0);

   // Setup interrupt
   setup_int();

   // Just loop
   while  ( 1 ) {
#ifdef NO_INTERRUPT
       ay_wyz_play();
       msleep(40);
#endif
   }
}
```

## Converting WYZTracker output to usable files

WYZTracker outputs two files: `tune.mus` and `tune.mus.asm`. Both of these files should be copied into your project.

`tune.mus.asm` needs to be modified to turn it into a format that can be used by z88dk. So open the file using an editor.

At the moment, the first thing you need to do is to change all `DW` to `defw` and all `DB` to `defb`. Then at the top of the file paste the following:

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