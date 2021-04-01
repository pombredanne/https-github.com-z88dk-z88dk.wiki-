# UNDER CONSTRUCTION

To aid conversion of Sam C programs to z88DK's [SAM Coupé Platform](https://github.com/z88dk/z88dk/wiki/Platform--Sam-Coupe.md) this table represents the libraries and functions offered in SamC and their equivelents in the Z88DK libraries.

Features that are particular to the platform are called out, not all of these may be available yet.

  | SAM C Library | Type | Function | Sam Specific | Description | Z88DK Function | Z88DK Library | Comments | 
  | ------------- | ---- | -------- | ------------ | ----------- | -------------- | ------------- | -------- |  
  | system.h | int  | avail(); | ? | Return free memory |  |  |  |  
  | system.h | void | disable(); |  | Disable interrupts | intrinsic_di() | intrinsic.h |  |  
  | system.h | int | dpeek(unsigned a); | ? | Double PEEK |  |  |  |  
  | system.h | void | dpoke(unsigned a, int b); | ? | Double POKE |  |  |  |  
  | system.h | void | enable(); |  | Enable interrupts | intrinsic_ei() | intrinsic.h |  |  
  | system.h | int | getsp(); | ? | Get Stack pointer |  |  |  |  
  | system.h | int | gettime(); | ? | Return frame counter |  |  |  |  
  | system.h | void | pause(int c); |  | Wait for frame interrupt(s) | intrinsic_halt() | intrinsic.h | Equivelent of pause(1); |  
  | system.h | int  | callcode (int a,int b,int d,int h,unsigned adr); | Y | Call machine code routine |  |  |  |  
  | system.h | int | escape(); | Y | Detect Escape key pressed |  |  |  |  
  | system.h | unsigned  | inp(unsigned p); |  | Read IN port | intrinsic_ini | intrinsic.h |  |  
  | system.h | unsigned  | outp(unsigned p,int v); |  | Write OUT port | intrinsic_outi | intrinsic.h |  |  
  | system.h | char | peek(unsigned a); |  | PEEK |  |  |  |  
  | system.h | void | poke(unsigned a, char b); |  | POKE |  |  |  |  
  | misc.h | int | blitz(...); | Y | BASIC Blitz command  |  |  |  |  
  | misc.h | int | button(void ); | Y | Mouse Button status|  |  |  |  
  | misc.h | int | getgraphmode(void ); | Y | Return current screen mode |  |  |  |  
  | misc.h | int | mdriver(void ); | Y | Is mouse driver loaded? |  |  |  |  
  | misc.h | void | outtextxy(int x, int y, char *s); | Y | Print string at x,y |  |  |  |  
  | misc.h | void | setpalette(int num, int color); | Y | Duplicate of palette? | sam_set_palette | sam.h |  |  
  | misc.h | int | xmouse(void ); | Y | Mouse X coordinates |  |  |  |  
  | misc.h | int | ymouse(void ); | Y | Mouse Y coordinates |  |  |  |  
  | misc.h | void | circle(int x, int y, int radius); |  | Duplicate from graphics.c |  |  |  |  
  | misc.h | enum | GCode { PLOT=1, DRAWTO, CIRCLE, OVER, PEN, CLS, PAUSE }; |  |  |  |  |  |  
  | misc.h | int | getpixel(int x, int y); |  | POINT command |  |  |  |  
  | misc.h | #define  | outtext puts |  |  |  |  |  |  
  | misc.h | #define  | putpixel(a,b,c) {pen(c);plot(a,b);} |  |  |  |  |  |  
  | misc.h | extern  | REGS regs; |  |  |  |  |  |  
  | misc.h | void | setallpalette(PALETTE *p); |  |  |  |  |  |  
  | misc.h | void | setbkcolor(int color); |  | Clone of PAPER? |  |  |  |  
  | misc.h | void | setcolor(int color); |  | Clone of PEN? |  |  |  |  
  | misc.h | void | setdrawmode(int a); |  | Clone of Over? |  |  |  |  
  | misc.h | typedef  | struct { unsigned char _A_; unsigned _BC, _DE, _HL, _IX, _IY;} REGS; |  |  |  |  |  |  
  | misc.h | typedef  | struct { unsigned char numcolor[16]; } PALETTE; |  |  |  |  |  |  
  | misc.h | int | user(unsigned addr); |  | What is this? |  |  |  |  
  | conio.h | void | blocks(int b); | Y | BASIC Blocks toggle 1/0 (UDG Graphics for foreign chars) |  |  |  |  
  | conio.h | void | csize(int x,int y); | Y | BASIC CSIZE |  |  |  |  
  | conio.h | void | window(int a,int b,int c,int d); | Y | Text Window create |  |  |  |  
  | conio.h | void | border(int b); |  |  |  |  |  |  
  | conio.h | void | bright(int b); |  |  |  |  |  |  
  | conio.h | void | flash(int f); |  |  |  |  |  |  
  | conio.h | void | inverse(int i); |  |  |  |  |  |  
  | conio.h | void | over(int o); |  | Duplicate with SETOVER? |  |  |  |  
  | conio.h | void | paper(int p); | y | Text/Graphics  background | textbackground() |  |  |  
  | conio.h | void | pen(int i); | y | Text/Graphics colour | textcolor() |  |  |  
  | conio.h | void | tab(int t); |  | Tab size |  |  |  |  
  | graphics.h | void | triangle(int x0, int y0, int x1, int y1, int x2, int y2); | ? | Pointless? |  |  |  |  
  | graphics.h | void | allpalette(int pal); | Y | Set all palette entries | sam_set_palette | sam.h |  |  
  | graphics.h | void | close_scr(int scr); | Y | Close screen |  |  |  |  
  | graphics.h | void | cls(int a); | Y | CLS different screen areas (0=screen/1=window) |  |  |  |  
  | graphics.h | void | display(int scr); | Y | Display screen |  |  |  |  
  | graphics.h | void | fatpix(int a); | Y | BASIC FATPIX for MODE 3 pixels, xrg=256 |  |  |  |  
  | graphics.h | unsigned  | grab(int x, int y, int w, int l); | Y | Sprite Grab |  |  |  |  
  | graphics.h | void | mode(int m); | Y | Screen Mode |  |  |  |  
  | graphics.h | void | open_scr(int scr, int mode); | Y | Open new screen |  |  |  |  
  | graphics.h | void | palette(int pos, int col); | Y | Set palette entries | sam_load_palette | sam.h |  |  
  | graphics.h | void | put(int x, int y, int mode); | Y | Sprite Put |  |  |  |  
  | graphics.h | void | roll  (int x, int y, int width, int length, int direct, int size); | Y | ROLL   commend |  |  |  |  
  | graphics.h | void | scroll(int x, int y, int width, int length, int direct, int size); | Y | SCROLL commend |  |  |  |  
  | graphics.h | int | setpattern(int p); | Y | Fill pattern |  |  |  |  
  | graphics.h | void | border(int a); | y | Duplicate |  |  |  |  
  | graphics.h | void | box(int x, int y, int w, int l); |  |  |  |  |  |  
  | graphics.h | void | circle(int x, int y, int r); |  |  |  |  |  |  
  | graphics.h | void | drawto(int x, int y); |  |  |  |  |  |  
  | graphics.h | void | fill(int x, int y, int mode); |  |  |  |  |  |  
  | graphics.h | int | getx(); |  | Current X |  |  |  |  
  | graphics.h | int | gety(); |  | Current Y |  |  |  |  
  | graphics.h | void | line(int x0, int y0, int x1, int y1); |  |  |  |  |  |  
  | graphics.h | void | moveto(int x, int y); |  |  |  |  |  |  
  | graphics.h | void | paper(int p); |  | Duplicate |  |  |  |  
  | graphics.h | void | pen(int i); |  | Duplicate |  |  |  |  
  | graphics.h | void | plot(int x, int y); |  |  |  |  |  |  
  | graphics.h | void | setover(int m); |  | BASIC OVER |  |  |  |  
  | string2.h | char | * itod(int nbr, char *str, int sz); |  |  |  |  |  |  
  | string2.h | char | * itoo(int nbr, char *str, int sz); |  |  |  |  |  |  
  | string2.h | char | * itou(int nbr, char *str, int sz); |  | Duplicate? |  |  |  |  
  | string2.h | char | * itox(int nbr, char *str, int sz); |  |  |  |  |  |  
  | string2.h | int | dtoi(char *decstr, int nbr); |  |  |  |  |  |  
  | string2.h | void | left(char *s); |  |  |  |  |  |  
  | string2.h | int | otoi(char *octstr, int nbr); |  |  |  |  |  |  
  | string2.h | void | reverse(char *s); |  |  |  |  |  |  
  | string2.h | int | utoi(char *decstr, int nbr); |  |  |  |  |  |  
  | string2.h | int | xtoi(char *hexstr, int nbr); |  |  |  |  |  |  
  | string.h | char | * memchr(char *s, int c, unsigned width); |  |  |  |  |  |  
  | string.h | void | * memcpy(char *a,char *b,unsigned c); |  |  |  |  |  |  
  | string.h | void | * memset(char *a,int b,unsigned c); |  |  |  |  |  |  
  | string.h | char | * stpcpy(char *dest, const char *src); |  |  |  |  |  |  
  | string.h | char | * strcat(char *a,char *b); |  |  |  |  |  |  
  | string.h | char | * strchr(char *a,int b); |  |  |  |  |  |  
  | string.h | char | * strcmp(char *a,char *b); |  |  |  |  |  |  
  | string.h | char | * strcpy(char *a,char *b); |  |  |  |  |  |  
  | string.h | char | * stricmp(char *a,char *b); |  |  |  |  |  |  
  | string.h | char | * strlwr(char *s); |  |  |  |  |  |  
  | string.h | char | * strncat(char *a,char *b,unsigned c); |  |  |  |  |  |  
  | string.h | char | * strncmp(char *a,char *b,unsigned c); |  |  |  |  |  |  
  | string.h | char | * strncpy(char *a,char *b,unsigned c); |  |  |  |  |  |  
  | string.h | char | * strnicmp(char *a,char *b,unsigned c); |  |  |  |  |  |  
  | string.h | char | * strnset(char *s, int ch, unsigned w); |  |  |  |  |  |  
  | string.h | char | * strrchr(char *a,int b); |  |  |  |  |  |  
  | string.h | char | * strset(char *s, int ch); |  |  |  |  |  |  
  | string.h | char | * strstr(char *a,char *b); |  |  |  |  |  |  
  | string.h | char | * strupr(char *s); |  |  |  |  |  |  
  | string.h | int | memcmp(char *a,char *b,unsigned c); |  |  |  |  |  |  
  | stdlib.h | int | outprn(int c); | ? | Text output value to printer |  |  |  |  
  | stdlib.h | int | rand(); | ? | Return Random Number  |  |  |  |  
  | stdlib.h | void | sort(char *base,int offset, int size, int items, int (*comp)(), int (*swap)()); | ? | Sort function |  |  |  |  
  | stdlib.h | void | gdump(); | Y | Bitmap output screento printer |  |  |  |  
  | stdlib.h | int | is512kb(); | Y | Return memory size |  |  |  |  
  | stdlib.h | void | nosound(); | Y | SAA1099 off |  |  |  |  
  | stdlib.h | void | sound(...); | Y | SAA1099 byte |  |  |  |  
  | stdlib.h | void | tdump(); | Y | Text output to printer |  |  |  |  
  | stdlib.h | char | * itoa(int value, char *string, int radix); |  | Duplicate |  |  |  |  
  | stdlib.h | void | abort(int c); |  | Duplicate  |  |  |  |  
  | stdlib.h | int | abs(int x); |  |  |  |  |  |  
  | stdlib.h | int | atexit(int fnc); |  |  |  |  |  |  
  | stdlib.h | int | atoi(int s); |  | Duplicate  |  |  |  |  
  | stdlib.h | void | beep(int duration, int pitch); |  | ZX Beeper |  |  |  |  
  | stdlib.h | void | exit(int a); |  | Duplicate  |  |  |  |  
  | stdlib.h | int | max (int a, int b); |  |  |  |  |  |  
  | stdlib.h | int | min (int a, int b); |  |  |  |  |  |  
  | stdlib.h | void | srand(int seed); |  |  |  |  |  |  
  | stdlib.h | void | swap(unsigned width, unsigned from, unsigned to); |  |  |  |  |  |  
  | stdio.h | const unsigned  | heaplen_; | ? | Return Heap length |  |  |  |  
  | stdio.h | const unsigned  | stklen_; | ? | Return Stack Length |  |  |  |  
  | stdio.h | char | * input(int line, int column, int max); | Y | BASIC INPUT at x,y of length |  |  |  |  
  | stdio.h | void | abort(int errcode); | Y | Exit(errorlevel) |  |  |  |  
  | stdio.h | void | assert(int e); | Y |  |  |  |  |  
  | stdio.h | void | at(int line, int column); | Y | Move text cursor to  X/Y |  |  |  |  
  | stdio.h | #define |  clrscr() clscr() |  |  |  |  |  |  
  | stdio.h | void | * calloc(unsigned n, unsigned size); |  |  |  |  |  |  
  | stdio.h | char | * fgets(char *string, int max, int stream); |  |  |  |  |  |  
  | stdio.h | void | * free(unsigned block); |  |  |  |  |  |  
  | stdio.h | char | * gets(char *string); |  |  |  |  |  |  
  | stdio.h | char | * itoa(int value, char *string, int radix); |  |  |  |  |  |  
  | stdio.h | char | * itou(int value, char *string, int radix); |  | Duplicate? |  |  |  |  
  | stdio.h | void | * malloc(unsigned size); |  |  |  |  |  |  
  | stdio.h | char | * MEMPTR_;    // pointer to first free byte |  |  |  |  |  |  
  | stdio.h | char | * skip(char *string); |  |  |  |  |  |  
  | stdio.h | int | atoi(char *s, int radix); |  |  |  |  |  |  
  | stdio.h | void | clscr(void ); |  |  |  |  |  |  
  | stdio.h | void | exit(int c); |  |  |  |  |  |  
  | stdio.h | int | fgetc(int strm); |  |  |  |  |  |  
  | stdio.h | int | fprintf(...); |  |  |  |  |  |  
  | stdio.h | void | fputc(int c, int strm); |  |  |  |  |  |  
  | stdio.h | void | fputs(char *s, int strm); |  |  |  |  |  |  
  | stdio.h | int | fscanf(...); |  |  |  |  |  |  
  | stdio.h | int | getc( void ); |  |  |  |  |  |  
  | stdio.h | int | getch( void ); |  |  |  |  |  |  
  | stdio.h | int | getchar( void ); |  |  |  |  |  |  
  | stdio.h | int | getche( void ); // as per getch(), but with echo |  |  |  |  |  |  
  | stdio.h | int | isdigit(int c); |  |  |  |  |  |  
  | stdio.h | int | isspace(int c); |  |  |  |  |  |  
  | stdio.h | int | kbhit( void ); |  |  |  |  |  |  
  | stdio.h | void | print(char *s); |  |  |  |  |  |  
  | stdio.h | int | printf(...);   // this six functions is VARIADIC !!! |  |  |  |  |  |  
  | stdio.h | void | putc(int c); |  |  |  |  |  |  
  | stdio.h | void | putchar(int c); |  |  |  |  |  |  
  | stdio.h | void | puts(char *s); |  |  |  |  |  |  
  | stdio.h | int | scanf(...); |  |  |  |  |  |  
  | stdio.h | int | sprintf(...); |  |  |  |  |  |  
  | stdio.h | int | sscanf(...); |  |  |  |  |  |  
  | stdio.h | void | stream(int stream); |  |  |  |  |  |  
  | stdio.h | unsigned  | strlen(char *s); |  | Duplicate with String? |  |  |  |  
  | stdio.h | void | ungetc(int c); |  |  |  |  |  |  
  | ctype.h | char | isalnum(int c); |  |  |  |  |  |  
  | ctype.h | char | isalpha(int c); |  |  |  |  |  |  
  | ctype.h | char | isascii(int c); |  |  |  |  |  |  
  | ctype.h | char | iscntrl(int c); |  |  |  |  |  |  
  | ctype.h | char | isgraph(int c); |  |  |  |  |  |  
  | ctype.h | char | islower(int c); |  |  |  |  |  |  
  | ctype.h | char | isprint(int c); |  |  |  |  |  |  
  | ctype.h | char | ispunct(int c); |  |  |  |  |  |  
  | ctype.h | char | isupper(int c); |  |  |  |  |  |  
  | ctype.h | char | isxdigit(int c); |  |  |  |  |  |  
  | ctype.h | char | toascii(int c); |  |  |  |  |  |  
  | ctype.h | char | tolower(int c); |  |  |  |  |  |  
  | ctype.h | char | toupper(int c); |  |  |  |  |  |  
 