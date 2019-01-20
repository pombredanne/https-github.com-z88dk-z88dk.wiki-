In addition to providing the standard C library, the classic library provides a number of cross platform APIs and features that can be used to aid portability across the targets.


## ANSI vt100 console

## Generic console (vt52)

The [generic console](Classic-GenericConsole) console driver provides a uniform set of escape codes to control text positioning, attributes etc. It is integrated into `<stdio.h>` and if not enabled as the default can be selected with the command line `--generic-console`.

## Inkey keyboard (hardware keyboard scanning)

Hardware keyboard scanning bypasses the target's ROM and directly polls the keyboard. This allows the detection of multiple keypresses and hence allows the creation of more complex games. The various routines are defined in [`<input.h>`](Classic---Input-library)

## Joystick support

Targets generally support at least some form of joystick support. Polling of the joystick is exposed using the `joystick()` function in `<games.h>`

There are the following effective levels of support:

* Two keyboard joysticks using ROM functions: Only a single direction can be registered at once
* Multiple keyboard joysticks using inkey driver: Multiple directions supported
* Hardware joystick support: Multiple directions supported

## Sound support

The classic library supports both a 1 bit [`<sound.h>`](Classic-1-bit-sound) and a PSG [`<psg.h>`](Classic---PSG-Library) based sound interface. 

The 1 bit library defines a collection of sound effects in addition to a tone generator.

## Monochrome graphics

A variety of drawing primitives and higher level functions are supplied by [`<graphics.h>`](Classic-Monochrome-Graphics). 


## Interrupt hook
