# PSG (psg.h)

| | |
|-|-|
 | Header     | [{z88dk}/include/psg.h](https://raw.githubusercontent.com/z88dk/z88dk/master/include/psg.h)    |        
 | Source     | [{z88dk}/z88dk/libsrc/psg](https://github.com/z88dk/z88dk/tree/master/libsrc/psg/)                     |
 | Include    | #include `<psg.h>`             |                                                                          
 | Linking    | n/a                          |                                                                          
 | Compile    | n/a                          |                                                                          
 | Comments   |                              |                                                                          


Very basic PSG interface library. It provides the channel activation and frequency conversion maths.

A minimal interface for the Commodore 128's SID is included.


# sound API


## psgT(hz)

Convert a frequency value to pulse count for the PSG data


## set_psg (reg, val)

Send data to the psg.    "sound(reg, value)" is a valid alias.


## get_psg (reg)

Read the PSG register.   Not supported on SID, use the SID specific calls.


## psg_init()

Init the PSG (reset sound etc..)   Not supported on SID.


## psg_tone()

Set a given tone for the channel (0-2)


## psg_noise (period)

Set the global noise period


## psg_volume(channel, volume)

Set channel's volume   (not channel restricted on SID)


## psg_envelope(waveform, period, channels)

Set the volume envelope of number \a waveform, with the given period, on a group of channels (ORed bits)


## psg_channels(tone_channels, noise_channels)

Set noise or tone generation on a group of channels (ORed bits).  Not supported on SID.


## psg_tone_channels()

Get the group of channels currently generating tone (ORed bits).   Not supported on SID.


## psg_noise_channels()

extern unsigned char __LIB__ psg_tone_channels();

Get the group of channels currently generating noise (ORed bits).   Not supported on SID.

