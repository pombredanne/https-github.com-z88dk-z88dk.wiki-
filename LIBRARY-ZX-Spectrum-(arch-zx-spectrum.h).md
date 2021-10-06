 |            |                                                                                                                     | 
 | ---------- | ------------------------------------------------------------------------------------------------------------------- | 
 | Header     | [{z88dk}/arch/zx/spectrum.h](https://raw.githubusercontent.com/z88dk/z88dk/master/include/arch/zx/spectrum.h)               | 
 | Source     | [{z88dk}/libsrc/target/zx](https://github.com/z88dk/z88dk/tree/master/libsrc/target/zx/)                              | 
 | Include    | #include `<arch/zx.h>`                                                                                             |
 | Linking    | n/a                                                                                                                 |
 | Compile    | n/a                                                                                                                 |
 | Supported  | [ZX Spectrum](platform/zx), [TS 2068](platform/ts2068) (partially)                                                  |  
 | Comments   | n/a                                                                                                                 |

The ZX Spectrum library contains functions specific to the Sinclair Spectrum family machines.

# Constants

## Attributes

 | INK_BLACK   | 0x00 | 
 | ---------   | ---- | 
 | INK_BLUE    | 0x01 | 
 | INK_RED     | 0x02 | 
 | INK_MAGENTA | 0x03 | 
 | INK_GREEN   | 0x04 | 
 | INK_CYAN    | 0x05 | 
 | INK_YELLOW  | 0x06 | 
 | INK_WHITE   | 0x07 | 

 | PAPER_BLACK   | 0x00 | 
 | -----------   | ---- | 
 | PAPER_BLUE    | 0x08 | 
 | PAPER_RED     | 0x10 | 
 | PAPER_MAGENTA | 0x18 | 
 | PAPER_GREEN   | 0x20 | 
 | PAPER_CYAN    | 0x28 | 
 | PAPER_YELLOW  | 0x30 | 
 | PAPER_WHITE   | 0x38 | 

 | BRIGHT | 0x40 | 
 | ------ | ---- | 
 | FLASH  | 0x80 | 



## BASIC language keyword tokens

These are the defined constants for the BASIC KEYWORDS

 | RND       | TK_RND       | 165 | 
 | ---       | ------       | --- | 
 | INKEY$    | TK_INKEYS    | 166 | 
 | PI        | TK_PI        | 167 | 
 | FN        | TK_FN        | 168 | 
 | POINT     | TK_POINT     | 169 | 
 | SCREEN$   | TK_SCREENS   | 170 | 
 | ATTR      | TK_ATTR      | 171 | 
 | AT        | TK_AT        | 172 | 
 | TAB       | TK_TAB       | 173 | 
 | VAL$      | TK_VALS      | 174 | 
 | CODE      | TK_CODE      | 175 | 
 | VAL       | TK_VAL       | 176 | 
 | LEN       | TK_LEN       | 177 | 
 | SIN       | TK_SIN       | 178 | 
 | COS       | TK_COS       | 179 | 
 | TAN       | TK_TAN       | 180 | 
 | ASN       | TK_ASN       | 181 | 
 | ACS       | TK_ACS       | 182 | 
 | ATN       | TK_ATN       | 183 | 
 | LN        | TK_LN        | 184 | 
 | EXP       | TK_EXP       | 185 | 
 | INT       | TK_INT       | 186 | 
 | SQR       | TK_SQR       | 187 | 
 | SGN       | TK_SGN       | 188 | 
 | ABS       | TK_ABS       | 189 | 
 | PEEK      | TK_PEEK      | 190 | 
 | IN        | TK_IN        | 191 | 
 | USR       | TK_USR       | 192 | 
 | STR$      | TK_STRS      | 193 | 
 | CHR$      | TK_CHRS      | 194 | 
 | NOT       | TK_NOT       | 195 | 
 | BIN       | TK_BIN       | 196 | 
 | OR        | TK_OR        | 197 | 
 | AND       | TK_AND       | 198 | 
 | < =       | TK_LEQ       | 199 | 
 | > =       | TK_GEQ       | 200 | 
 | `< >`       | TK_NEQ       | 201 | 
 | LINE      | TK_LINE      | 202 | 
 | THEN      | TK_THEN      | 203 | 
 | TO        | TK_TO        | 204 | 
 | STEP      | TK_STEP      | 205 | 
 | DEF FN    | TK_DEF_FN    | 206 | 
 | CAT       | TK_CAT       | 207 | 
 | FORMAT    | TK_FORMAT    | 208 | 
 | MOVE      | TK_MOVE      | 209 | 
 | ERASE     | TK_ERASE     | 210 | 
 | OPEN      | TK_OPEN      | 211 | 
 | CLOSE     | TK_CLOSE     | 212 | 
 | MERGE     | TK_MERGE     | 213 | 
 | VERIFY    | TK_VERIFY    | 214 | 
 | BEEP      | TK_BEEP      | 215 | 
 | CIRCLE    | TK_CIRCLE    | 216 | 
 | INK       | TK_INK       | 217 | 
 | PAPER     | TK_PAPER     | 218 | 
 | FLASH     | TK_FLASH     | 219 | 
 | BRIGHT    | TK_BRIGHT    | 220 | 
 | INVERSE   | TK_INVERSE   | 221 | 
 | OVER      | TK_OVER      | 222 | 
 | OUT       | TK_OUT       | 223 | 
 | LPRINT    | TK_LPRINT    | 224 | 
 | LLIST     | TK_LLIST     | 225 | 
 | STOP      | TK_STOP      | 226 | 
 | READ      | TK_READ      | 227 | 
 | DATA      | TK_DATA      | 228 | 
 | RESTORE   | TK_RESTORE   | 229 | 
 | NEW       | TK_NEW       | 230 | 
 | BORDER    | TK_BORDER    | 231 | 
 | CONTINUE  | TK_CONTINUE  | 232 | 
 | CONT      | TK_CONT      | 232 | 
 | DIM       | TK_DIM       | 233 | 
 | REM       | TK_REM       | 234 | 
 | FOR       | TK_FOR       | 235 | 
 | GO TO     | TK_GO_TO     | 236 | 
 | GO SUB    | TK_GO_SUB    | 237 | 
 | INPUT     | TK_INPUT     | 238 | 
 | LOAD      | TK_LOAD      | 239 | 
 | LIST      | TK_LIST      | 240 | 
 | LET       | TK_LET       | 241 | 
 | PAUSE     | TK_PAUSE     | 242 | 
 | NEXT      | TK_NEXT      | 243 | 
 | POKE      | TK_POKE      | 244 | 
 | PRINT     | TK_PRINT     | 245 | 
 | PLOT      | TK_PLOT      | 246 | 
 | RUN       | TK_RUN       | 247 | 
 | SAVE      | TK_SAVE      | 248 | 
 | RANDOMIZE | TK_RANDOMIZE | 249 | 
 | RANDOMIZE | TK_RAND      | 249 | 
 | IF        | TK_IF        | 250 | 
 | CLS       | TK_CLS       | 251 | 
 | DRAW      | TK_DRAW      | 252 | 
 | CLEAR     | TK_CLEAR     | 253 | 
 | RETURN    | TK_RETURN    | 254 | 
 | COPY      | TK_COPY      | 255 | 

# Configuration diagnostics

Since there are many different hardware configurations, having some sort of diagnosis can greatly help the developer to build compatible software.

Unluckily some of those routines won't work on the standard  2A,  3 or Pentagon models.  The zx_model function can help in those cases.

## int zx_128mode()

TRUE if in Spectrum 128 mode

## int zx_issue3()

TRUE if your spectrum model is issue 3 or more.
Works by checking the keyboard behaviour.

## int zx_type()

Identifies your Spectrum type

 | 0 | 48K    | 
 | - | ---    | 
 | 1 | 128K   | 
 | 2 | TS2068 | 


## int zx_model()

Identifies your Spectrum model:

 | 0 | unknown                          | 
 | - | -------                          | 
 | 1 | 48K                              | 
 | 2 | 128K or  +2                      | 
 | 3 | +2A or Pentagon                  | 
 | 4 | +3                               | 
 | 5 | +2A/ +3 with bus fixed for games | 
 | 6 | TS2068                           | 

## int zx_basic_length()

Computes the size of the BASIC program in memory (if any).

## int zx_var_length()

Computes the current size of the BASIC variables area.

## int zx_printer()

Returns TRUE if the ZX Printer is attached.

## int zx_soundchip()

Returns TRUE if some sort of Yamaha soundchip is connected as for the Spectrum 128.

## int zx_timexsound()

## zx_fullerstick()

## int zx_kempstonmouse()

## int zx_kempston()

Returns TRUE if a Kempston Joystick interface is connected.

## int zx_iss_stick()

Returns TRUE if an ISS Joystick interface is connected.

## int zx_multiface()


## int zx_disciple()

Returns TRUE if a Disciple /  D interface is connected

## int zx_plus3fdc()

Returns TRUE if the Spectrum  3 floppy disk controller is present.

## int zx_trd()

Returns TRUE if TRDOS is active.

## int zx_extsys()

Returns TRUE if if some sort of system extension has moved the BASIC program.

## int zx_basemem()

Gives the size of the base memory (normally 16K or 48K).

# Interfacing to the ZX Spectrum BASIC


## int zx_goto(int line)

Runs a BASIC portion, starting at the given line.
The returned value is zero or the BASIC error code, if any.

## int zx_getstr(char variable, char *value)

Load the specified BASIC string variable into *value.

## void zx_setstr(char variable, char *value)

Copies the string pointed by *value in the specified BASIC string variable, creating it if necessary.

## int zx_getint(char *variable)

Return the integer value for the specified BASIC numeric variable.

## void zx_setint(char *variable, int value)

Copies "value" to the specified BASIC variable.

*variable should be set with the BASIC variable name.

## double zx_getfloat(char *variable)

Return the floating point value for the specified BASIC numeric variable.

## void zx_setfloat(char *variable, float value)

Copies "value" (double type format) to the specified BASIC variable, creating it if necessary.

*variable must be set with the BASIC variable name.


#  Tape I/O 

## Standard tape header structure

    struct zxtapehdr {
             unsigned char type;
             char    name[10];
             size_t length;
             size_t address;
             size_t offset;
    };


## int tape_save(char *name, size_t loadstart,void *start, size_t len)

## int tape_save_block(void *addr, size_t len, unsigned char type)

## int tape_load_block(void *addr, size_t len, unsigned char type)


# Input Devices: Keyboard, Joystick and Mice

Reading input is organized by the [ input library](library/input ).  Subroutines specific to the spectrum and consistent with the input library are found here.

## Joystick Functions



### uint in_JoyFuller(void)

### uint in_JoyKempston(void)

### uint in_JoySinclair1(void)

### uint in_JoySinclair2(void)

### uint in_JoyTimex1(void)

### uint in_JoyTimex2(void)


## Mouse Functions

### AMX Mouse

The AMX mouse is designed to operate in the z80's interrupt mode 2.  To use the AMX Mouse you must set up im2 mode (see the [ im2 library](library/interrupts ) for details) and, with interrupts disabled, call *in_MouseAMXInit()* with two interrupt vectors upon which the two AMX interrupt service routines will be installed for you.  These interrupt vectors must be even since the AMX hardware contains a Z80-PIO chip which can only store even vectors.  Any two even vectors will do as long as they don't conflict with any other hardware.  On a Spectrum, vectors 0 and 2 are as good as any.

The library also requires that the program declares a few global variables in RAM to hold position data:

```
unsigned int in_AMXcoordX, in_AMXcoordY, in_AMXdeltaX, in_AMXdeltaY;
```

The call to *in_MouseAMXInit()* initializes these variables to sensible values.

*in_AMXdeltaX* and *in_AMXdeltaY* control the mouse sensitivity.  They are 8.8 fixed point indicating the number of pixels to adjust the mouse coordinates by for each unit of distance the mouse is moved.  *in_MouseAMXInit()* initializes them to 0x80 (1 unit of physical motion = 1 pixel of movement on screen).

#### void in_MouseAMXInit(uchar xvector, uchar yvector)

#### void in_MouseAMX(uchar *buttons, uint *xcoord, uint *ycoord)

#### void in_MouseAMXSetPos(uint xcoord, uint ycoord)


### Kempston Mouse

The library requires that the program declare a few global variables in RAM to hold position data:

```
uchar in_KempcoordX, in_KempcoordY, in_KemprawX, in_KemprawY;
```

These variables are initialized when *in_MouseKempInit()* is called.

#### void in_MouseKempInit(void)

#### void in_MouseKemp(uchar *buttons, uint *xcoord, uint *ycoord)

#### void in_MouseKempSetPos(uint xcoord, uint ycoord)


# Display Functions

## Borrowed From Sinclair Basic

### void zx_border(uchar colour)

Set the border colour.  Use one of the attribute constants defined at the start of the header (BLACK, BLUE, etc).  Changing the border colour involves an OUT to port $fe, which also controls the internal speaker.  If your program contains beeper output this may result in an audible click if the OUT happens to toggle the speaker output bit.  The library cannot avoid this as it is not known what the last output to the speaker was; to avoid this the program can keep track of the last bit output to the speakerand OR it with the desired border colour in a call such as *border(RED | last_speaker_bit);*  For most programs this is a non-issue.

### uint zx_attr(uchar row, uchar col)

Return the attribute on screen of the character coordinate given.  Works just like the Basic ATTR() command.

### uint zx_screenstr(uchar row, uchar col)

Return the ascii character code for the character printed on screen at the character coordinate given, if any.  If an ascii character is not recognized at that position zero is returned.  This function is similar to Basic's SCREEN$() command but will also match characters if the bitmap on screen is a mixture of inverted and non-inverted patterns (the basic command will only match if the character is either inverted or not-inverted on screen, but not a mixture).  Currently does not match the spectrum's block graphics.

## Display Address Manipulators

A collection of functions designed to make access to the Spectrum's display file easy and fast.

In the following, *screen address* refers to the pixel address within the display file (16384-22527) and *attribute address* refers to an address in the attributes area (22528-23295).

```
Function names are constructed from the following atoms:

saddr = screen address
aaddr = attribute address
 
px    = pixel x coordinate (0..255)
py    = pixel y coordinate (0..191)
pxy   = pixel (x,y) coordinate

cx    = character x coordinate (0..31)
cy    = character y coordinate (0..23)
cyx   = character (y,x) coordinate - ordering borrowed from Sinclair Basic
```

So for example:


*  **zx_saddr2cy(saddr)** will return the character y coordinate corresponding to the given screen address

*  **zx_saddr2aaddr(saddr)** will return the attribute address corresponding to the given screen address

*  **zx_pxy2aaddr(px,py)** will return the attribute address corresponding to the given (x,y) pixel coordinate

Some functions will return with carry flag set if coordinates or addresses move out of bounds.  In these cases the special z88dk keywords *iferror()* and *ifnerror()* can be used to test the carry flag and determine if results are valid.  Functions that do this are documented as such below.

### Screen Address Manipulators (pixels)

#### uchar *zx_cyx2saddr(uchar row, uchar col)

#### uchar *zx_cy2saddr(uchar row)

#### uchar *zx_pxy2saddr(uchar xcoord, uchar ycoord, uchar *mask)

#### uchar *zx_py2saddr(uchar ycoord)

#### uint zx_saddr2cx(void *pixeladdr)

#### uint zx_saddr2cy(void *pixeladdr)

#### uint zx_saddr2px(void *pixeladdr, uchar mask)

#### uint zx_saddr2py(void *pixeladdr)

#### uchar *zx_saddr2aaddr(void *pixeladdr)

#### uchar *zx_saddrcdown(void *pixeladdr)

#### uchar *zx_saddrcleft(void *pixeladdr)

#### uchar *zx_saddrcright(void *pixeladdr)

#### uchar *zx_saddrcup(void *pixeladdr)

#### uchar *zx_saddrpdown(void *pixeladdr)

#### uchar *zx_saddrpleft(void *pixeladdr, uchar *mask)

#### uchar *zx_saddrpright(void *pixeladdr, uchar *mask)

#### uchar *zx_saddrpup(void *pixeladdr)

### Attribute Address Manipulators (attr)

#### uchar *zx_cyx2aaddr(uchar row, uchar col)

#### uchar *zx_cy2aaddr(uchar row)

#### uchar *zx_pxy2aaddr(uchar xcoord, uchar ycoord)

#### uchar *zx_py2aaddr(uchar ycoord)

#### uint zx_aaddr2cx(void *attraddr)

#### uint zx_aaddr2cy(void *attraddr)

 
#### uint zx_aaddr2px(void *attraddr)

#### uint zx_aaddr2py(void *attraddr)

#### uchar *zx_aaddr2saddr(void *attraddr)

#### uchar *zx_aaddrcdown(void *attraddr)

#### uchar *zx_aaddrcleft(void *attraddr)

#### uchar *zx_aaddrcright(void *attraddr)

#### uchar *zx_aaddrcup(void *attraddr)

