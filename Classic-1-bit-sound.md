# Sound `<sound.h>`

Many targets supported by z88dk support a 1 bit speaker, toggling this bit can result in sound effects. The classic library provides portable routines to create sound FX and play music.



## Precanned sound effects

The `bit_fx(int)` function family provides ready made sound effects. The sounds have been split into groups of 8.

```
bit_fx(0)	Short hi-pitch decreasing "gulp/fall" sound
bit_fx(1)	Fast increasing scale
bit_fx(2)	Fast increasing squeak, mid duration, high pitch
bit_fx(3)	Crash sound, mid duration
bit_fx(4)	Fast increasing squeak/jump, short duration
bit_fx(5)	Short two tones clackson sound
bit_fx(6)	Two tones mid/high pitch bright beep
bit_fx(7)	Medium duration moving down, then up

bit_fx2(0)	Sort of electronic synth, mid-low tone, mid duration
bit_fx2(1)	Mid duration low tone, beating waves effect
bit_fx2(2)	Quick low pitch electronic buzz/noise
bit_fx2(3)	Long two tones truck clackson
bit_fx2(4)	Two tones increasing sound, electr. fx, mid duration
bit_fx2(5)	Two tones bounce sound
bit_fx2(6)	Alt. two tones bounce sound
bit_fx2(7)	Long low explosion sound

bit_fx3(0)	Quick noisy high pitch buzz / break noise
bit_fx3(1)	Longer very noisy high pitch buzz
bit_fx3(2)	Very quick low electronic noise / explosion / drum
bit_fx3(3)	Short low explosion sound
bit_fx3(4)	Electronic zap, mid duration
bit_fx3(5)	Noisy high pitch short increasing sound
bit_fx3(6)	Low level noise (AM radio noise)
bit_fx3(7)	Quick high pitch tremolo sound

bit_fx4(0)	Very quick squeaky sound
bit_fx4(1)	Tape rewind effect, long duration
bit_fx4(2)	Fast squeak/bounce/duck sound
bit_fx4(3)	Fall-down, mid duration
bit_fx4(4)	High pitch fall-down sound, long duration
bit_fx4(5)	Short high pitch slightly noisy decreasing sound
bit_fx4(6)	Short jump sound
bit_fx4(7)	Very quick duck squeak
```

## Playing notes



`void bit_synth(int duration, int frequency1, int frequency2, int frequency3, int frequency4)`

Polyphony and multitimbric effects

`void bit_beep(int duration, int period)`

Standard beeper: the pitch is defined with the "period" parameter, but is influenced by the CPU speed; the higher the value, the lower the tone.



`void bit_frequency(double_t duration, double_t frequency)`

Play a note at the specified frequency. The CPU speed doesn't affect the frequency, but some floating point is involved (you need lo link in the proper math library at compile time).

`void bit_play(unsigned char melody[])`

Just play a song. For example:

```
bit_play("2A--A-B-CDEFGAB5C+");
```

## Creating your own sound effects

`void bit_open()`

Initialize the sound hardware.

`void bit_close()`

Do whatever is needed to restore the sound hardware into a quiet position.

`void bit_clock()`

Just move the sound bit.. a tiny tick will be heard. Can be useful to create sound effects in software loops (i.e. engine noise for a game).