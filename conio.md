# Conio (conio.h)

 | Header     | [{z88dk}/include/conio.h](https///raw.githubusercontent.com/z88dk/z88dk/master/include/conio.h)    |
 | -----------------------------------------------------------------------------------------------------------------
 | Source     |                              |                                                                      
 | Include    | #include `<conio.h>`           |                                                                      
 | Linking    |                              |                                                                      
 | Compile    | +(target)ansi                |                                                                      
 | Comments   | Most of the console output sequences work correctly only in VT-ANSI mode |                          

MS-DOS compatibility library.


# CONIO API

## Console Input

    kbhit
    getch
    getche
    cgets
    cscanf


## Console Output

    putch
    cputs
    cprintf
    gotoxy
    settextcolor
    textcolor
    textbackground
    textattr
    clrscr
    clreol


