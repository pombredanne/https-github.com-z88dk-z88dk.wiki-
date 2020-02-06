# Introduction

WYZPlayer is a cross-platform music and sound effects player for AY-8910 based computers.

## Machine support

- MSX
- ZX Spectrum

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
#include <string.h>
#include <im2.h>
#include <intrinsic.h>
#include <interrupt.h>
#include <stdlib.h>
#include <psg/wyz.h>


extern wyz_song meson;

M_BEGIN_ISR(playmusic) {
   ay_wyz_play();
M_END_ISR
}


#ifdef __SPECTRUM__
void setup_interrupt() {
   intrinsic_di();
   memset(0xd300, 0xd4, 257);       // initialize 257-byte im2 vector table with all 0xd4 bytes
   im2_Init(0xd300);                // place z80 in im2 mode with interrupt vector table located at 0xd300
   bpoke(0xd4d4, 195);              // POKE jump instruction at address 0xd4d4 (interrupt service routine entry)
   wpoke(0xd4d5, playmusic);        // POKE isr address following the jump instruction
   intrinsic_ei();
}
#endif

#ifdef __MSX__
void setup_interrupt() {
   im1_init();
   im1_install_isr(playmusic);
}
#endif


void main() {
   // Load the tracker file
   ay_wyz_init(&mysong);
   // Play song 1 within the  file
   ay_wyz_start(0);

   // Setup interrupt
   setup_interrupt();

   // Just loop
   while  ( 1 ) {
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
