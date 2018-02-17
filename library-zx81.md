# ZX 81 LIBRARY (zx81.h)

    
    | Header     | [[https://raw.githubusercontent.com/z88dk/z88dk/master/include/zx81.h|{z88dk}/include/zx81.h]]  |
    | Source     | [[https://github.com/z88dk/z88dk/tree/master/libsrc/zx81/|{z88dk}/libsrc/zx81]]                 |
    | Include    | #include <zx81.h>                                                                               |
    | Linking    | n/a                                                                                             |
    | Compile    | n/a                                                                                             |
    | Supported  | [[platform:zx81|zx 81]]                                                                         |
    | Comments   | n/a                                                                                             |

The ZX 81 library contains functions specific to the Sinclair ZX81 / Timex 1000 family machines.



## Constants


### BASIC language keyword tokens

 |    |            |           | 
 | ---------   | -----------           | ----          | 
 | RND    | TK_RND      | 64  | 
 | INKEY$ | TK_INKEYS   | 65  | 
 | PI  | TK_PI    | 66  | 
 | " " | TK_DBLQUOTE | 192 | 
 | AT  | TK_AT    | 193 | 
 | TAB | TK_TAB   | 194 | 
 |CODE|TK_CODE|196| 
 |VAL|TK_VAL|197| 
 |LEN|TK_LEN|198|
 |SIN|TK_SIN|199|
 |COS|TK_COS|200|
 |TAN|TK_TAN|201|
 |ASN|TK_ASN|202|
 |ACS|TK_ACS|203|
 |ATN|TK_ATN|204|
 |LN|TK_LN|205|
 |EXP|TK_EXP|206|
 |INT|TK_INT|207|
 |SQR|TK_SQR|208|
 |SGN|TK_SGN|209|
 |ABS|TK_ABS|210|
 |PEEK|TK_PEEK|211|
 |USR|TK_USR|212|
 |STR$|TK_STRS|213|
 |CHR$|TK_CHRS|214|
 |NOT|TK_NOT|215|
 |* *|TK_STARSTAR|216|
 |OR|TK_OR|217|
 |AND|TK_AND|218|
 | < = |TK_LEQ|219|
 | > = |TK_GEQ|220|
 | < > |TK_NEQ|221|
 |THEN|TK_THEN|222|
 |TO|TK_TO|223|
 |STEP|TK_STEP|224|
 |LPRINT|TK_LPRINT|225|
 |LLIST|TK_LLIST|226|
 |STOP|TK_STOP|227|
 |SLOW|TK_SLOW|228|
 |FAST|TK_FAST|229|
 |NEW|TK_NEW|230|
 |SCROLL|TK_SCROLL|231|
 |CONTINUE|TK_CONTINUE|232|
 |CONT|TK_CONT|232|
 |DIM|TK_DIM|233|
 |REM|TK_REM|234|
 |FOR|TK_FOR|235|
 |GO TO|TK_GO_TO|236|
 |GO SUB|TK_GO_SUB|237|
 |INPUT|TK_INPUT|238|
 |LOAD|TK_LOAD|239|
 |LIST|TK_LIST|240|
 |LET|TK_LET|241|
 |PAUSE|TK_PAUSE|242|
 |NEXT|TK_NEXT|243|
 |POKE|TK_POKE|244|
 |PRINT|TK_PRINT|245|
 |PLOT|TK_PLOT|246|
 |RUN|TK_RUN|247|
 |SAVE|TK_SAVE|248|
 | RANDOMIZE | TK_RANDOMIZE | 249 |
 | RANDOMIZE | TK_RAND   | 249 |
 | IF     | TK_IF     | 250 |
 | CLS    | TK_CLS    | 251 |
 | UNPLOT | TK_UNPLOT | 252 |
 | CLEAR  | TK_CLEAR  | 253 |
 | RETURN | TK_RETURN | 254 |
 | COPY   | TK_COPY   | 255 |

## Configuration diagnostics


### int zx_basic_length()

Computes the size of the BASIC program in memory (if any).

### int zx_var_length()

Computes the current size of the BASIC variables area.




## Interfacing to the ZX 81 BASIC

The following functions open a powerful way to exchange data between the BASIC environment and to develop programs mixing BASIC, C and machine code modules.
The variable names are automatically translated from ASCII to the ZX81 char encoding, while the string content translator can, if necessary, be disabled to manage the ZX81 special characters.


### int zx_line(int line)

Runs a single BASIC line.
The returned value is zero or the BASIC error code, if any.



### int zx_getstr(char variable, char *value)

Load the specified BASIC string variable into *value.

The character codes are converted from ZX81 to ASCII.   (This behaviour can be changed with the //zx_asciimode// function).


### void zx_setstr(char variable, char *value)

Copies *value into a given BASIC string variable, creating it if necessary.

The character codes are converted from ASCII to ZX81. (This behaviour can be changed with the //zx_asciimode// function).

### int zx_getint(char *variable)

Return the integer value for the specified BASIC numeric variable.

### void zx_setint(char *variable, int value)

Copies "value" (int format) to the specified BASIC variable, creating it if necessary.
*variable must be set with the BASIC variable name.

### double zx_getfloat(char *variable)

Return the floating point value for the specified BASIC numeric variable.

### void zx_setfloat(char *variable, float value)

Copies "value" (floating point format) to the specified BASIC variable, creating it if necessary.
*variable must be set with the BASIC variable name.


#### Example

Interfacing the ZX 81 BASIC

This demonstration program computes the coordinates for a second level equation, then calls BASIC to plot the pixel.


First compile the program:

	
	zcc +zx81 -o zxbasic -create-app zxbasic.c
	

Then load "zxbasic.p" in your (real or emulated) zx81, and add the following BASIC lines:


        100 PLOT X,Y
        900 STOP


Now you can RUN the demo.
Try also changing the BASIC sub as follows:

	
        100 PRINT AT Y/2,X/2;"O"

----

    #include <stdio.h>
    
    main()
    {
    int x;
    
	for (x=-30; x<30; x++)
	{
    
		zx_setint ("Y",x*x/30);
		zx_setint ("X",32+x);
		zx_line (100); // 100 PLOT X,Y
    
	}
	// BASIC termination stub
	zx_line (900); // 900 STOP
    }


## Miscellaneous functions


### invtxt()

Invert the text mode screen



### mirrortxt()

Mirror screen horizontally in text mode: rightmost character is put at leftmost position and vice versa.




### zx_asciimode()

Activates / Deactivates the ZX81 <-> ASCII converter used in some output routine and interfacing to the BASIC strings.   The ZX81 libraries are set by default to "1" (on).

Note that the normal console output library is based on this converter: when it is disabled and wrong characters are put on the screen, the display handler might be confused, making your program crash.


### zx_ascii(unsigned char character)

ZX81 -> ASCII char conversion


### ascii_zx(unsigned char character)

ASCII -> ZX81 char conversion


### zx_fast()

Switch to FAST mode, not available if subtype=fast.
To gain speed with no screen flickering in HRG you can use blank()/noblank().

### zx_slow()

Switch BACK to SLOW mode, not available if subtype=fast.


### hrg_tune_left()

Hardware shifting of TV picture in HRG mode


### hrg_tune_right()

Hardware shifting of TV picture in HRG mode


### blank()

Hides the display picture with no flicker and lets the zx81 run faster.



### noblank()

Makes the video visible again, back from the 'blank' status.





# High resolution library related functions

There are three available versions of the High resolution libraries, **gfx81hr192** (WRX hardware mod) and **gfx81arx192** (ARX816 hardware mod) providing a full 256x192 graphics map, and the smaller but faster **gfx81hr64**/**gfx81arx64** which is capable of 256x64 pixels.
**gfx81mt64** (248x192) and **gfx81mt192** (248*64) do the same with The Memotech HRG while **g007** supports the G007 board (it needs to be initialized from BASIC with the "SLOW 4" command).
Look for more informations on how to activate the WRX style HRG engine at the [ZX81 platform description page](Platform---Sinclair-ZX81).



![](images/zx81_mt_hrg.jpg)

{{:library:zx81_mt_hrg.jpg|}} {{:library:wall-chroma-81.jpg|}}


The pictures above show the "microman" demo running on a Memotech HRG board and "wall" on a recent colour expansion (Chroma-81, 2014), comatible with the WRX mode.

## base_graphics global variable

This global variable holds the current graphics map.
It can be forced to a specific value in the C source with the #pragma directive, in example:

    #pragma output hrgpage = 36096

By default (when no hrgpage is specified) the program is compiled with a builtin relocator for the stack, targetted to a 16K model.
For 8K models it is possible to modify the relocator with the following definition:

    #pragma output MEM8K

It is also possible to specify the graphics page location at binary level, by POKEing locations 16518/16519.
If the  " #pragma output hrgpage = "  directive is used, an alternate map can still be POKEd, but the automatic relocation engine is excluded.




### _clg_hr()

Clear graphics page.  It is equal to clg().


### hrg_off()

Disable the High Resolution Graphics mode and go back to text mode.


### hrg_on()

Enable the High Resolution Graphics mode and go back to text mode.


### invhrg()

Invert the video output if in High Resolution mode.
This is done without affecting the data in the video memory.


## copytxt(int ovmode)

Copies the text page to the HRG screen, permitting to mix text and graphics behaviours.
The following modes can be used:


|txt_and|"white ink"|

|txt_and_cpl|"white ink" and invert|

|txt_or|normal black on white output|

|txt_xor|xor (sort of sprite effect)|

|txt_or_r|shift 1 pixel left before printing, useful for **BOLD** or similar effects|

|txt_or_l|shift 1 pixel right before printing, as above|

|txt_and_r|as above, but using "white ink"|

|txt_and_l|as above|

