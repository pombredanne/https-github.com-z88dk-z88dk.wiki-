
![](images/platform/sharplogo.gif)


# Quick start (MZ-80K/700 series)

    zcc +mz -lm -o adventure -create-app -pragma-define:REGISTER_SP=0xd000 adv_a.c

-or-

    zcc +mz -subtype=ansi -lm -o adventure -create-app -pragma-define:REGISTER_SP=0xd000 adv_a.c


This command will build a .MZT tape image (also known as MZF or M12).

It is possible to choose a full VT/ANSI emulation or a tiny console support.



# Sharp MZ-80B / MZ-2000 series

This group of systems features a different way to access to the I/O devices (the 'mzansi' library will not work) and needs to load an initial program (the IPL ROM normally 'boots' the MONITOR and the BASIC interpreter from tape).  The tape transfer speed differs from the other models.

At the moment the differences are managed by the 'appmake tool', by patching the resulting code and encoding the audio output in the proper way:

    zcc +mz -create-app -Cz--audio -Cz--mz80b -Cz--src -Cz700 -Cz--dst -Cz80b adv_a.c

To load the program the SB-1510 monitor must be in memory, this is normally part of the BASIC boot tape, just enter the 'MONIT' command to activate it, load the program with the 'L' command, then type 'J' to jump (default address is 1200 hexadecimal).

Note that only the WAV audio version will be altered by the patching process, the intermediate .MZT file will not work with the MZ80B.

# Appmake extras

The appmake tool is capable to create an audio version of the output program, ready for the original cassette player.   

    zcc +mz -subtype=ansi -lm -o adventure -create-app -Cz--audio -Cz--fast adv_a.c


Appmake can also be run in "dumb" mode to generate the corresponding audio track of external programs, in the following example a tweaked fast audio track is generated for 'program.mzt':

    C:\>appmake +mz --dumb --audio --fast -b program.mzt
    

More options:

--mz80b  (faster mode use in the mz80b)

--turbo  (tested on MZ700 only) (turbo tape loader, then faster data)

--fast   speeds up loading time, *experimental* if in use with "--mz80b"


Overview:

 | Machine | (normal) | --fast   | --turbo  | --mz80b | 
 | ------- | -------- | ------   | -------  | ------- | 
 | MZ80K   | untested | untested | untested | no      | 
 | MZ80A   | untested | untested | untested | no      | 
 | MZ80B   | no       | no       | no       | yes(2)  | 
 | MZ700   | yes      | yes      | yes      | no      | 
 | MZ800   | yes(1)   | no?      | no       | no      | 

(1) MZ800 only in MZ700 mode

(2) for the MZ80B, patching is needed.


	
	Example on how to use the experimental patching options to convert external binary programs.
	Valid targets are 2000,80b,700,sos (being the support for the latter very weak)
	
	>
	>appmake +mz --dumb --src sos --dst 80b -b LIFEGAME.mzt
	
	Info: name found in header: LIFEGAME
	
	Info: file type:         1
	Info: program location:  $3000
	Info: binary block size: $300
	Info: start address:     $3000
	
	Info: Patching location 3015, opcode 'cd', address $1fe8->$8db
	Info: Patching location 3015, opcode 'cd', address $1fe8->$8db
	Info: Patching location 302c, opcode 'cd', address $1fcd->$562
	Info: Patching location 302c, opcode 'cd', address $1fcd->$562
	Info: Patching location 3085, opcode 'cd', address $1fbe->$5d8
	Info: Patching location 309c, opcode 'cd', address $1fbe->$5d8
	Info: Patching location 318c, opcode 'cd', address $1fe8->$8db
	Info: Patching location 318c, opcode 'cd', address $1fe8->$8db
	Info: Patching location 320d, opcode 'c3', address $1ffa->$872
	Info: Patching location 3267, opcode 'cd', address $1ff4->$916
	Info: Patching location 3267, opcode 'cd', address $1ff4->$916
	Info: Patching location 3281, opcode 'cd', address $1ff4->$916
	Info: Patching location 3281, opcode 'cd', address $1ff4->$916
	Info: Patching location 329b, opcode 'cd', address $1ff4->$916
	Info: Patching location 329b, opcode 'cd', address $1ff4->$916
	Info: Patching location 32b8, opcode 'cd', address $1ff4->$916
	Info: Patching location 32b8, opcode 'cd', address $1ff4->$916
	Info: Patching location 32c3, opcode 'cd', address $1fd0->$871
	Info: Patching location 32c3, opcode 'cd', address $1fd0->$871


Many thanks to Fabrizio Freudiger for his help testing the appmake results on the real computers.

# Hints on the Sharp MZ emulators

There's a known issue with an old Sharp MZ emulator (MZ700WIN).
If you experience problems, try running the program under a different emulator or an updated version.


# Links


[Disk image editor](http://web.archive.org/web/query?q=wayback_server%3A25+type%3Aurlquery+url%3Ahttp%253A%252F%252Fwww.geocities.co.jp%252FSiliconValley-Sunnyvale%252F2521%252Fbin%252Fdownload%252Fmzd88ctl.zip&count=150000&start_page=1)

[Andy Collins' random jottings: Retrochallenge 2015 with the MZ-700 and z88dk](http://www.randomorbit.co.uk/?cat=97)

[Using z88dk to get MZ-700 compatible code](http://www.randomorbit.co.uk/?cat=37)

[z88dk on the MZ-1500 (japanese)](https://sites.google.com/view/mz1500page/tips/z88dk%E3%82%92%E4%BD%BF%E3%81%86)
