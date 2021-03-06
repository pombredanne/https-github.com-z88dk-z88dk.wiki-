z88dk has code to support the following sound generators:

* AY819x
* SN76489
* SAA1099
* One bit beeper sound

## AY819x support

Two tracker players are supported by the 819x based machines:

* [WYZ Player](Classic-WYZ-Player) `<psg/wyz.h>` [Example](https://github.com/z88dk/z88dk/tree/master/examples/sound/wyz)
* VortexTracker `<psg/vt2.h>` [Example](https://github.com/z88dk/z88dk/tree/master/examples/sound/vt2)

## SN76489

The SN76489 have a port of [PSGLib](https://github.com/sverx/PSGlib) available via `<psg/PSGlib.h>`. [Example](https://github.com/z88dk/z88dk/tree/master/examples/sound/psglib)

## SAA1099

For the SAA1099, etracker is available via `<psg/etracker.h>`. [Example](https://github.com/z88dk/z88dk/tree/master/examples/sound/etracker)

Additionally, [WYZ Player](Classic-WYZ-Player) `<psg/wyz.h>` is available via emulation.

## One bit sound

Sound effects and sounds are available via `<sound.h>`