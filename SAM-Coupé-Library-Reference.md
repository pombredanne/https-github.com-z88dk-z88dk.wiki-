To aid conversion of Sam C programs to z88DK's [SAM Coupé Platform](https://github.com/z88dk/z88dk/wiki/Platform--Sam-Coupe.md) this table represents the libraries and functions offered in SamC and their equivalents in the Z88DK libraries.

Features that are particular to the platform are called out, not all of these may be available yet or fully working.

A series of routines are being written to test these functions and any deviation recorded in the Status column.

Missing functions are rated in priority, low priority functions may exist as part of other libraries or easily replicated using other functions.

Original compiler and assembler source available [Sam C on GitHub](https://github.com/mkrivos/samcoupecode), [SAM C manual](http://www.samcoupe-pro-dos.co.uk/othersammanuals.html) and copies of the [demos, headers and Z80 assembler source of most libraries](https://github.com/dandoore/samclibs).

Total functions: 158

Missing/Unaccounted for: 62

**Note:** Do not update this table, it is generated off-site.

| SAM C Library | Type | Function | Sam Specific | Addional Details | Matching | Matching Lib | Priority | Status |  
| ------------- | ---- | -------- | ------------ | ---------------- | -------- | ------------ | -------- | ------ |
| conio.h | void | blocks(int b); | Y | BASIC Blocks toggle UDG Graphics for foreign chars(1=on/0=off) |  |  | 1 - High | MISSING |  
| conio.h | void | csize(int x,int y); | Y | BASIC CSIZE (x can be 6 or 8, y can be 8,9 or 16) |  |  | 1 - High | MISSING |  
| conio.h | void | over(int o); | ? | Set OVER status for text (0-Normal 1-XOR 2-OR 3-AND) |  |  | 1 - High | MISSING - extension to ZX OVER command? | 
| graphics.h | unsigned  | grab(int x, int y, int w, int l); | ? | Sprite Grab |  |  | 1 - High | MISSING |  
| graphics.h | void | put(int x, int y, int mode); | ? | Sprite Put x,y,mode (1-Inverse 2-OR 4-ovewrite(default) 5-using mask) |  |  | 1 - High | MISSING |  
| graphics.h | void | roll  (int x, int y, int width, int length, int direct, int size); | ? | ROLL  command - scroll section of the screen wrapping around |  |  | 1 - High | MISSING |  
| graphics.h | void | scroll(int x, int y, int width, int length, int direct, int size); | Y | SCROLL  command - scroll section of the screen no wrapping |  |  | 1 - High | MISSING |  
| graphics.h | void | setover(int m); | Y | Graphics writing mode (0 -Normal, 1-XOR, 2-OR, 3-AND) |  |  | 1 - High | MISSING |  
| misc.h | int | button(void ); |  | Read Mouse Button value (left/right/both) |  |  | 1 - High | MISSING - Add to input.h | 
| misc.h | int | getpixel(int x, int y); |  | POINT command, return pixel pot colour from x,y or attribute from MODE1/2 | point() | graphics.h | 1 - High | #1748 SAM point(x,y) needs to return pot colour of pixel in high colour modes, not just binary set/not set |  
| misc.h | int | mdriver(void ); |  | Mouse Init /init (return false if no driver) |  |  | 1 - High | MISSING - Add to input.h |  
| misc.h | int | xmouse(void ); |  | Read Mouse X |  |  | 1 - High | MISSING - Add to input.h |  
| misc.h | int | ymouse(void ); |  | Read Mouse Y |  |  | 1 - High | MISSING - Add to input.h |  
| stdlib.h | int | is512kb(); | Y | Return true if 256kb memory expansion fitted |  |  | 1 - High | MISSING |  
| stdlib.h | void | nosound(); | Y | Turn SAA1099 off (SOUND 0x1c,0) |  |  | 1 - High | MISSING |  
| stdlib.h | void | sound(...); | Y | Send SAA1099 byte array of register/values (SOUND r1,v1;r2,v2;r3,v3…..) example: SOUND 0x2,255;0xa,195;&0x11,4;0x14,4;0x1c,1 gives continuous tone |  |  | 1 - High | MISSING |  
| system.h | int | escape(); | ? | Detect Escape key pressed (returns 1 if pressed) |  |  | 1 - High | MISSING |  
| conio.h | void | bright(int b); |  | Assert BRIGHT by selecting pot +8 of current (1=on/0=off), BRIGHT 8 or 16 is like ZX  'transparent' |  |  | 2 - Med | MISSING - refactor  zx_setattrbright(b)? |  
| conio.h | void | flash(int f); |  | Assert FLASH in MODE1/2 only (1=on/0=off) FLASH  8 or 16 is like ZX 'transparent' |  |  | 2 - Med | MISSING - refactor zx_setattrflash(b)? |  
| conio.h | void | inverse(int i); |  | Invert text pen/paper (1=on/0=off) |  |  | 2 - Med | MISSING - refactor zx_setattrinverse(b) ? |  
| conio.h | void | window(int a,int b,int c,int d); | ? | Text Window create |  |  | 2 - Med | MISSING |  
| graphics.h | void | close_scr(int scr); | Y | Close screen (number) |  |  | 2 - Med | MISSING |  
| graphics.h | void | cls(int a); | Y | CLS different screen areas (0=screen/1=window) |  |  | 2 - Med | MISSING - Support for clearing text window |  
| graphics.h | void | display(int scr); | Y | Display screen (number) |  |  | 2 - Med | MISSING |  
| graphics.h | void | open_scr(int scr, int mode); | Y | Open new screen (number, MODE) |  |  | 2 - Med | MISSING |  
| stdio.h | char | * fgets(char *string, int max, int stream); |  | Crippled FileIO - only works for streams in SAMC | *fgets() | stdio.h | 2 - Med | No FileIO yet |  
| stdio.h | int | fgetc(int strm); |  | Crippled FileIO - only works for streams in SAMC | fgetc() | stdio.h | 2 - Med | No FileIO yet |  
| stdio.h | int | fprintf(...); |  | Crippled FileIO - only works for streams in SAMC | fprintf() | stdio.h | 2 - Med | No FileIO yet | 
| stdio.h | void | fputc(int c, int strm); |  | Crippled FileIO - only works for streams in SAMC | fputc() | stdio.h | 2 - Med | No FileIO yet |  
| stdio.h | void | fputs(char *s, int strm); |  | Crippled FileIO - only works for streams in SAMC | fputs() | stdio.h | 2 - Med | No FileIO yet |  
| stdio.h | int | fscanf(...); |  | Crippled FileIO - only works for streams in SAMC | fscanf() | stdio.h | 2 - Med | No FileIO yet |  
| stdio.h | void | stream(int stream); | Y | Crippled FileIO - only works for streams in SAMC |  |  | 2 - Med | No FileIO yet |  
| conio.h | void | tab(int t); | Y | Move text cursor position right by tab value t |  |  | 3 - Low | MISSING |  
| graphics.h | void | fill(int x, int y, int mode); |  | Fill area - Does not support fill with pattern | fill() | graphics.h | 3 - Low | Missing Fill support |  
| graphics.h | int | setpattern(int p); | Y | Fill pattern |  |  | 3 - Low | MISSING | 
| graphics.h | void | triangle(int x0, int y0, int x1, int y1, int x2, int y2); | ? | Pointless - can be replicated in vanilla C |  |  | 3 - Low | MISSING |  
| misc.h | int | blitz(...); | Y | BASIC Blitz function |  |  | 3 - Low | MISSING |  
| misc.h | int | getgraphmode(void ); | Y | Return current MODE value |  |  | 3 - Low | MISSING IOCTL_GENCON_GET_MODE needed? | 
| misc.h | void | outtextxy(int x, int y, char *s); | ? | Print string at x,y - Code does a gotoxy() followed |  |  | 3 - Low | MISSING |  
| misc.h | extern  | REGS regs; | ? | What is this? |  |  | 3 - Low | MISSING |  
| misc.h | int | user(unsigned addr); | ? | What is this? |  |  | 3 - Low | MISSING |  
| stdio.h | char | * input(int line, int column, int max); | Y | BASIC INPUT at x,y of length (non standard) |  |  | 3 - Low | MISSING |  
| stdio.h | char | * MEMPTR_;    // pointer to first free byte | ? | Pointer used by malloc and calloc? Same as free? |  |  | 3 - Low | MISSING |  
| stdio.h | char | * skip(char *string); |  | Skip whitespace in string? Not in manual |  |  | 3 - Low | MISSING |  
| stdio.h | const unsigned  | heaplen_; | ? | Holds Heap length |  |  | 3 - Low | MISSING |  
| stdio.h | const unsigned  | stklen_; | ? | Holds Stack Length |  |  | 3 - Low | MISSING |  
| stdlib.h | void | gdump(); |  | Bitmap output entire screen to printer |  |  | 3 - Low | MISSING - Refactor zx_hardcopy()? |  
| stdlib.h | int | outprn(int c); |  | Text output value to printer |  |  | 3 - Low | MISSING - Refactor zx_lprintc() |  
| stdlib.h | void | tdump(); |  | Text output entire screen to printer |  |  | 3 - Low | MISSING - Refactor zx_print_buf()? |  
| string2.h | char | * itod(int nbr, char *str, int sz); | ? | Convert integer number as string to decimal as string - needed? |  |  | 3 - Low | MISSING |  
| string2.h | char | * itoo(int nbr, char *str, int sz); | ? | Convert integer number as string to octal as string - needed? |  |  | 3 - Low | MISSING |  
| string2.h | char | * itou(int nbr, char *str, int sz); | ? | Convert integer number as string to unsigned as string - needed? |  |  | 3 - Low | MISSING |  
| string2.h | char | * itox(int nbr, char *str, int sz); | ? | Convert integer number as string to hex as string - needed? |  |  | 3 - Low | MISSING |  
| string2.h | int | dtoi(char *decstr, int nbr); | ? | Convert dec number as string to integer as string - needed? |  |  | 3 - Low | MISSING |  
| string2.h | void | left(char *s); | ? | Left adjusts and terminates a string -needed? |  |  | 3 - Low | MISSING |  
| string2.h | int | otoi(char *octstr, int nbr); | ? | Convert oct number as string to integer as string - needed? |  |  | 3 - Low | MISSING |  
| string2.h | void | reverse(char *s); | ? | Reverses character order of NULL terminated string and leaves it terminated |  |  | 3 - Low | MISSING |  
| string2.h | int | utoi(char *decstr, int nbr); | ? | Convert unsigned number as string to integer asstring - needed? |  |  | 3 - Low | MISSING |  
| string2.h | int | xtoi(char *hexstr, int nbr); | ? | Convert hex number as string to integer as string - needed? |  |  | 3 - Low | MISSING |  
| system.h | int  | callcode (int a,int b,int d,int h,unsigned adr); | Y | Call machine code routine (A, BC, DE, HL,Address) |  |  | 3 - Low | MISSING |  
| system.h | int | getsp(); | ? | Get Stack pointer value - needed? |  |  | 3 - Low | MISSING |  
| system.h | int | gettime(); | ? | Return frame counter SVAR FRAMES 0X5c78 |  |  | 3 - Low | MISSING |  
| conio.h | void | border(int b); |  | Set border colour (pot) | bordercolor() | conio.h |  |  |  
| conio.h | void | paper(int p); |  | Text/Graphics  background (pot) (MODE1/2 can set 16 'transparent' and 17  'contrast' like ZX) | textbackground() | conio.h |  |  |  
| conio.h | void | pen(int i); |  | Text/Graphics colour(pot) (MODE1/2 can set 16 'transparent' and 17  'contrast' like ZX) | textcolor() | conio.h |  |  |  
| ctype.h | char | isalnum(int c); |  |  | isalnum(int) | ctype.h |  |  | 
| ctype.h | char | isalpha(int c); |  |  | isalpha(int) | ctype.h |  |  |  
| ctype.h | char | isascii(int c); |  |  | isascii(int) | ctype.h |  |  |  
| ctype.h | char | iscntrl(int c); |  |  | iscntrl(int) | ctype.h |  |  |  
| ctype.h | char | isgraph(int c); |  |  | isgraph(int) | ctype.h |  |  |  
| ctype.h | char | islower(int c); |  |  | islower(int) | ctype.h |  |  |  
| ctype.h | char | isprint(int c); |  |  | isprint(int) | ctype.h |  |  |  
| ctype.h | char | ispunct(int c); |  |  | ispunct(int) | ctype.h |  |  |  
| ctype.h | char | isupper(int c); |  |  | isupper(int) | ctype.h |  |  |  
| ctype.h | char | isxdigit(int c); |  |  | isxdigit(int) | ctype.h |  |  |  
| ctype.h | char | toascii(int c); |  |  | toascii(int) | ctype.h |  |  |  
| ctype.h | char | tolower(int c); |  |  | tolower(int) | ctype.h |  |  |  
| ctype.h | char | toupper(int c); |  |  | toupper(int) | ctype.h |  |  |  
| graphics.h | void | allpalette(int pal); |  | Set all palette entries | sam_load_palette | arch/sam.h |  |  |  
| graphics.h | void | box(int x, int y, int w, int l); |  | Draw an unfilled box | drawb() | graphics.h |  |  |  
| graphics.h | void | circle(int x, int y, int r); |  | Draw a circle | circle() | graphics.h |  |  |  
| graphics.h | void | drawto(int x, int y); |  | Draw from current position to x,y | drawto() | graphics.h |  |  |  
| graphics.h | void | fatpix(int a); | Y | FATPIX mode for MODE 3 pixels (also sets xrg to 256) | sam_fatpix | arch/sam.h |  | Request #1772 |  
| graphics.h | int | getx(); |  | Current X graphics position | getx() | graphics.h |  |  |  
| graphics.h | int | gety(); |  | Current Y graphics position | gety() | graphics.h |  |  |  
| graphics.h | void | line(int x0, int y0, int x1, int y1); |  | Draw linefrom x1,y1 to x2,y2 | draw() | graphics.h |  |  |  
| graphics.h | void | mode(int m); |  | Set screen MODE (MODE) | console_ioctl() | ioctl.h |  |  |  
| graphics.h | void | moveto(int x, int y); |  | Move graphics plot position | setpos() | graphics.h |  |  |  
| graphics.h | void | palette(int pos, int col); |  | Set palette entries (pot, value) | sam_set_palette | arch/sam.h |  |  | 
| graphics.h | void | plot(int x, int y); |  | Plot pixel at x,y | plot() | graphics.h |  |  | 
| stdio.h | #define |  clrscr() clscr() |  |  | clrscr() | conio.h |  |  |  
| stdio.h | void | * calloc(unsigned n, unsigned size); |  |  | *calloc() | malloc.h |  |  |  
| stdio.h | void | * free(unsigned block); |  |  | free() | malloc.h |  |  |  
| stdio.h | char | * gets(char *string); |  |  | *gets() | stdio.h |  |  |  
| stdio.h | char | * itoa(int value, char *string, int radix); |  |  | *itoa() | stdlib.h |  |  |  
| stdio.h | void | * malloc(unsigned size); |  |  | *malloc() | malloc.h |  |  |  
| stdio.h | void | assert(int e); |  |  | assert() | assert.h |  |  |  
| stdio.h | void | at(int line, int column); |  | Move text cursor to  X/Y | gotoxy() | conio.h |  |  |  
| stdio.h | int | atoi(char *s, int radix); |  |  | atoi() | stdlib.h |  |  |  
| stdio.h | void | clscr(void ); |  | Clear screen | clrscr() | conio.h |  |  |  
| stdio.h | void | exit(int c); |  | Exit with specified errorlevel | exit() | stdlib.h |  |  |  
| stdio.h | int | getc( void ); |  | Get character | getc() | stdio.h |  |  |  
| stdio.h | int | getch( void ); |  | Get character | getch() | conio.h |  |  |  
| stdio.h | int | getchar( void ); |  | Get character | getchar() | stdio.h |  |  |  
| stdio.h | int | getche( void ); // as per getch(), but with echo |  | Get character with echo to screen? | getche() | conio.h |  |  |  
| stdio.h | int | isdigit(int c); |  |  | isdigit(int) | ctype.h |  |  |  
| stdio.h | int | isspace(int c); |  |  | isspace(int) | ctype.h |  |  |  
| stdio.h | int | kbhit( void ); |  |  | kbhit() | conio.h |  |  |  
| stdio.h | void | print(char *s); |  | printf() a string | printf() | stdio.h |  | No FileIO yet |  
| stdio.h | int | printf(...);   // this six functions are variable !!! |  |  | printf() | stdio.h |  |  |  
| stdio.h | void | putc(int c); |  | Put character diretcly onto stdout | putc() | stdio.h |  |  |  
| stdio.h | void | putchar(int c); |  |  | putchar() | stdio.h |  |  |  
| stdio.h | void | puts(char *s); |  |  | puts() | stdio.h |  |  |  
| stdio.h | int | scanf(...); |  |  | scanf() | stdio.h |  |  |  
| stdio.h | int | sprintf(...); |  |  | sprintf() | stdio.h |  |  |  
| stdio.h | int | sscanf(...); |  |  | sscanf() | stdio.h |  |  |  
| stdio.h | unsigned  | strlen(char *s); |  |  | strlen() | string.h |  |  |  
| stdio.h | void | ungetc(int c); |  |  | ungetc() | stdio.h |  |  |  
| stdlib.h | int | abs(int x); |  |  | abs() | stdlib.h |  |  |  
| stdlib.h | int | atexit(int fnc); |  |  | atexit() | stdlib.h |  |  |  
| stdlib.h | void | beep(int duration, int pitch); |  |  | bit_beep() | sound.h |  |  |  
| stdlib.h | int | max (int a, int b); |  | Returns greater of two integers | max() | bdscio.h |  |  |  
| stdlib.h | int | min (int a, int b); |  | Returns lesser of two integers | min() | bdscio.h |  |  |  
| stdlib.h | int | rand(); |  | Return Random Number  | rand() | stdlib.h |  |  |  
| stdlib.h | void | sort(char *base,int offset, int size, int items, int (*comp)(), int (*swap)()); |  | Sort function | qsort_sccz80() | stdlib.h |  |  |  
| stdlib.h | void | srand(int seed); |  | Set random seed | srand() | stdlib.h |  |  |  
| stdlib.h | void | swap(unsigned width, unsigned from, unsigned to); |  | Swaps characters - part of sort routine | qsort_sccz80() | stdlib.h |  |  |  
| string.h | char | * memchr(char *s, int c, unsigned width); |  |  | *memchr() | string.h |  |  |  
| string.h | void | * memcpy(char *a,char *b,unsigned c); |  |  | *memcpy() | string.h |  |  |  
| string.h | void | * memset(char *a,int b,unsigned c); |  |  | *memset() | string.h |  |  |  
| string.h | char | * stpcpy(char *dest, const char *src); |  |  | *stpcpy() | string.h |  |  | 
| string.h | char | * strcat(char *a,char *b); |  |  | *strcat() | string.h |  |  |  
| string.h | char | * strchr(char *a,int b); |  |  | *strchr() | string.h |  |  |  
| string.h | char | * strcmp(char *a,char *b); |  |  | strcmp() | string.h |  |  |  
| string.h | char | * strcpy(char *a,char *b); |  |  | *strcpy() | string.h |  |  | 
| string.h | char | * stricmp(char *a,char *b); |  |  | stricmp() | string.h |  |  |  
| string.h | char | * strlwr(char *s); |  |  | *strlwr() | string.h |  |  |  
| string.h | char | * strncat(char *a,char *b,unsigned c); |  |  | *strncat() | string.h |  |  |  
| string.h | char | * strncmp(char *a,char *b,unsigned c); |  |  | strncmp() | string.h |  |  |  
| string.h | char | * strncpy(char *a,char *b,unsigned c); |  |  | *strncpy | string.h |  |  |  
| string.h | char | * strnicmp(char *a,char *b,unsigned c); |  |  | strnicmp() | string.h |  |  |  
| string.h | char | * strnset(char *s, int ch, unsigned w); |  |  | *strnset | string.h |  |  |  
| string.h | char | * strrchr(char *a,int b); |  |  | *strrchr() | string.h |  |  |  
| string.h | char | * strset(char *s, int ch); |  |  | *strset | string.h |  |  |  
| string.h | char | * strstr(char *a,char *b); |  |  | *strstr | string.h |  |  |  
| string.h | char | * strupr(char *s); |  |  | *strupr | string.h |  |  |  
| string.h | int | memcmp(char *a,char *b,unsigned c); |  |  | memcmp() | string.h |  |  |  
| system.h | int  | avail(); |  | Free memory that may be used by malloc and calloc | free() | malloc.h |  |  |  
| system.h | void | disable(); |  | Disable interrupts | intrinsic_di() | intrinsic.h |  |  |  
| system.h | int | dpeek(unsigned a); |  | PEEK 16 bit value (double byte) | wpoke() | stdlib.h |  |  |  
| system.h | void | dpoke(unsigned a, int b); |  | POKE 16 bit value (double byte) | wpeek() | stdlib.h |  |  |  
| system.h | void | enable(); |  | Enable interrupts | intrinsic_ei() | intrinsic.h |  |  |  
| system.h | unsigned  | inp(unsigned p); |  | Read IN port | inp() | stdlib.h |  |  |  
| system.h | unsigned  | outp(unsigned p,int v); |  | Write OUT port | outp() | stdlib.h |  |  |  
| system.h | void | pause(int c); |  | Wait until number of  frame interrupts received Equivelent of pause(1) for actual pausing in milliseconds use msleep() in stdlib.h makes more sense if specifing | intrinsic_halt() | intrinsic.h |  |  |  
| system.h | char | peek(unsigned a); |  | PEEK 8 bit value | bpoke() | stdlib.h |  |  |  
| system.h | void | poke(unsigned a, char b); |  | POKE 8 bit value | bpeek() | stdlib.h |  |  |  