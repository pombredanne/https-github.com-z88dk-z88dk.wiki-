
z88dk is able to compile programs for the "VZ 200 family".
Such group of computers includes the Dick Smith VZ200 and VZ300 the Video Technology Laser 210 and Laser 310, the Texet TX 8000 and the Salora Fellow.

The graphics mode is automatically switched on by calling "clg" or printing CHR$ 12 or calling the [vz_mode()](library/vz200) function.



# Quick start

    zcc +vz -lm -o adventure.vz adv_a.c

-or-

    zcc +vz -clib=ansi -lm -o adventure.vz adv_a.c


Using The "-subtype=basic" option a BASIC loader block is embedded in the CRT0 stub, so that the resulting binary file is ready for the "RUN" command.  By default an auto-run binary block is created.

To get a WAV version of the program it is necessary to add the "-create-app" and the "-Cz--audio" options.
To tweak the speed and gain little time "-Cz--fast" is available.

# Firmware problems

When printing characters using the firmware routines, the keyboard is also read, a character printed on screen and a beep emitted. To avoid this behaviour:

* Compile with the generic console (`-pragma-redirect:fputc_cons=fputc_cons_generic`)
* Consider using the "inkey" keyboard routines: (`-pragma-redirect:fgetc_cons=fgetc_cons_inkey -pragma-redirect:getk=getk_inkey`)

Using the options above, the firmware is avoided and your application will hopefully behave as expected.


# Appmake extras

Appmake is able to convert the compiled program from VZ to the newer CAS format; optionally a wav can be created for loading onto the original hardware, even in a slightly 'faster' mode.


To compile a program and generate a CAS and a WAV file:

    zcc +vz -create-app -o rpn rpn.c -Cz--audio

To use appmake as a dumb converter and speed-up a little the audio stream:

    appmake +vz -b airstrip.vz --audio --fast

# Old behaviour

Note: The "-subtype=bin" parameter is required to avoid extra headers.

	
	THIS IS THE OLD PROCEDURE  
	- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
	How pass the code to the VZ emulator.
	
	- Get the VZ emulator from:
	  http://www.powerup.com.au/~intertek/VZ200/vz.htm
	- Get the utilities, also
	- Set the environment variables correctly
	- Compile your code (to a.bin) using the "-subtype=bin" parameter 
	- Use the rbinary utility (Rbinary.exe a.bin a.vz)
	- Run the emulator
	- Type: POKE 30862,0:POKE 30863,128
	- Press F10, then 1 (Load Program)
	- Choose a.vz
	- Return to the emulator (press ENTER)
	- Type: X=USR(0)




# Links

http://bushy.vzalive.bangrocks.com/

http://www.vz200.org/bushy/

[Scrolly demo (YouTube Video)](https://www.youtube.com/watch?v=80nJ4RiR8xs)

[Flappy bird game code](https://github.com/gameblabla/flappybird_vz200/releases/tag/1.0) ..see also the [video](https://m.youtube.com/watch?v=mXtx4F2rmVg)

[Experimenting with the z88dk examples](https://m.youtube.com/watch?v=u8amUYLfi18)