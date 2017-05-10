# x11 (Xlib emulation)

 | Header     | [{z88dk}/include/X11](https///github.com/z88dk/z88dk/tree/master/include/X11)/*.h    |                       
 | ---------------------------------------------------------------------------------------------------                       
 | Source     | [{z88dk}/libsrc/graphics/x11](https///github.com/z88dk/z88dk/tree/master/libsrc/graphics/x11)               |
 | Include    | #include `<X11/Xlib.h>`        |                                                                               
 | Linking    | -lx11                        |                                                                               
 | Compile    | n/a                          |                                                                               
 | Comments   |                              |                                                                               


Starting from version 1.8 of z88dk we are providing an experimental library to emulate some basic behaviour fo the Xlib functions.
It isn't based on the network layer as the original library, and it implements only a tiny subset of the original functions but, being based on the "monochrome graphics" library, it is fairy portable to many of the supported targets.

This [example source program](http://z88dk.cvs.sourceforge.net/viewvc/z88dk/z88dk/examples/graphics/xample.c?revision=1.2&content-type=text%2Fplain) can be compiled and run with almost no changes for both the z88dk systems and a Unix X-Windows system.

The window background is saved and the window title, and the XPM icon are managed as expected.
The font loading is cleverly simulated, and the size is adapted by choosing between two base fonts.




### Support tools

An [AWK script](http://z88dk.cvs.sourceforge.net/z88dk/z88dk/support/X11/bdf2c.awk?view=markup) is provided to easily convert the X font in a form suitable for the library.


