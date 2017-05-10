# SOUND/BIT.H

 | Include    | #include `<sound/bit.h>`                                                                                              |                                                     
 | ----------------------------------------------------------------------------------------------------------------------------------                                                     
 | Header     | [{z88dk}/include/_DEVELOPMENT/sccz80/sound/bit.h](http://z88dk.cvs.sourceforge.net/viewvc/z88dk/z88dk/include/_DEVELOPMENT/sccz80/sound/bit.h?content-type=text%2Fplain) |
 | | [{z88dk}/include/_DEVELOPMENT/sdcc/sound/bit.h](http://z88dk.cvs.sourceforge.net/viewvc/z88dk/z88dk/include/_DEVELOPMENT/sdcc/sound/bit.h?content-type=text%2Fplain) |               
 | Source     | [{z88dk}/libsrc/_DEVELOPMENT/sound/bit](http://z88dk.cvs.sourceforge.net/viewvc/z88dk/z88dk/libsrc/_DEVELOPMENT/sound/bit/z80/)                     |                     

# SOUND EFFECTS

## void bit_beepfx(void *bfx)

## void bit_beepfx_di(void *bfx)

## void bit_fx(void *bfx)

## void bit_fx_di(void *bfx)


# TONE GENERATORS

## void bit_beep(uint16_t dur_ms, uint16_t freq_hz)

## void bit_beep_di(uint16_t dur_ms, uint16_t freq_hz)

## void bit_beep_raw(uint16_t cycles_num, uint16_t period_T)

## void bit_beep_raw_di(uint16_t cycles_num, uint16_t period_T)

## void bit_synth(uint16_t dur, uint16_t freq_1, uint16_t freq_2, uint16_t freq_3, uint16_t freq_4)

## void bit_synth_di(uint16_t dur, uint16_t freq_1, uint16_t freq_2, uint16_t freq_3, uint16_t freq_4)


# MUSIC PLAYERS

## char *bit_play(char *melody)

## char *bit_play_di(char *melody)

## void *bit_play_tritone(void *song)

## void *bit_play_tritone_di(void *song)


# MISCELLANEOUS

## void bit_click(void)

## void bit_click_di(void)


