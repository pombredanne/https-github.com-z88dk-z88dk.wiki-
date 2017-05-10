# SETJMP.H

 | Include    | #include `<stdlib.h>`                                                                                              |                         
 | -------------------------------------------------------------------------------------------------------------------------------                         
 | Header     | [{z88dk}/include/_DEVELOPMENT/sccz80/setjmp.h](http://z88dk.cvs.sourceforge.net/viewvc/z88dk/z88dk/include/_DEVELOPMENT/sccz80/setjmp.h) | 
 | | [{z88dk}/include/_DEVELOPMENT/sdcc/setjmp.h](http://z88dk.cvs.sourceforge.net/viewvc/z88dk/z88dk/include/_DEVELOPMENT/sdcc/setjmp.h) |                
 | Source     | [{z88dk}/libsrc/_DEVELOPMENT/setjmp](http://z88dk.cvs.sourceforge.net/viewvc/z88dk/z88dk/libsrc/_DEVELOPMENT/setjmp/)                     |

# SAVE CALLING ENVIRONMENT

## int setjmp(jmp_buf env)

# RESTORE CALLING ENVIRONMENT

## _Noreturn void longjmp(jmp_buf env, int val)

