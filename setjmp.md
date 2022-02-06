# SETJMP.H

 | | |                         
 | ---------- | ------------------------------------------------------------------------------------------------------------------ |                         
 | Include    | #include `<stdlib.h>`                                                                                              |                         
 | Header     | [{z88dk}/include/_DEVELOPMENT/sccz80/setjmp.h](https://raw.githubusercontent.com/z88dk/z88dk/master/include/_DEVELOPMENT/sccz80/setjmp.h) | 
 | | [{z88dk}/include/_DEVELOPMENT/sdcc/setjmp.h](https://raw.githubusercontent.com/z88dk/z88dk/master/include/_DEVELOPMENT/sdcc/setjmp.h) |                
 | Source     | [{z88dk}/libsrc/_DEVELOPMENT/setjmp](https://raw.githubusercontent.com/z88dk/z88dk/master/libsrc/_DEVELOPMENT/setjmp/)                     |

# SAVE CALLING ENVIRONMENT

## int setjmp(jmp_buf env)

# RESTORE CALLING ENVIRONMENT

## _Noreturn void longjmp(jmp_buf env, int val)

