 ====== STANDARD I/O (stdio.h) ======

 | Header     | not available yet                                                                                               |  
 | ------------------------------------------------------------------------------------------------------------------------------  
 | Source     | [{z88dk}/libsrc/stdio_new](https///github.com/z88dk/z88dk/tree/master/libsrc/stdio_new/)                     |     
 | Include    | #include `<stdio.h>`                                                                                              |  
 | Linking    | n/a                                                                                                             |  
 | Compile    | n/a                                                                                                             |  
 | Comments   | These library functions are compiled as part of each target’s implicit library.                                 |

These are IO functions defined as part of the C standard.

# Overview

In the C model, programs communicate with devices by reading and writing a sequence of characters through a construct called a **stream**.  A stream is a channel connecting the program to exactly one open file on a specific device.

A stream consists of an optional high level portion identified by a FILE* that sits on top of a mandatory low level portion that is directly connected to the device driver and is identified by a numerical file descriptor (fd).  Programs can choose to send and receive data from the device through the high level FILE* or directly through the low level fd, though most programmers will find using the FILE* interface more convenient most of the time as it supplies formatted input and output operations.

Although all data read or sent to the device through a FILE* is eventually transmitted to the device through the underlying fd, care must be taken if communication with a specific file is done through both the high level FILE* and the low level fd.  This is because the high level FILE* portion of the stream may be buffered, meaning data written may not have been immediately sent to the device driver following a write.  A subsequent write using the direct fd interface may actually have its data sent to the device driver prior to the previous buffered write using FILE*.  To avoid this situation, streams should be flushed (see *fflush()*) when switching from i/o using FILE* to i/o using fd to ensure that any data in high level buffers are sent to the device driver immediately.

A high level interface can be added to an existing fd by calling *fdopen()* and the underlying fd can be determined from an existing high level FILE* using *fileno()*.

Aside from read/write operations, stdio also provides a means to randomly access data within files using *fseek(), lseek(), rewind()*, a means to control the behaviour of the device using *ioctl()*, a means to detect errors with *feof(), ferror()* and a means to determine if the device is ready to send or receive data with *poll()*.

At program startup, three streams are opened for you.  They are:


*  *FILE *stdin, fd=0*.  Open for reading, typically connected to the keyboard.  For default program input.

*  *FILE *stdout, fd=1*.  Open for writing, typically connected to the primary display.  For default program output.

*  *FILE *stderr, fd=2*.  Open for writing, also typically connected to the primary display though guaranteed to be unbuffered.  For errors.

# To Do

** Insert stuff on writing device drivers, selecting devices for a program **

# API Description

[High Level I/O (FILE *)](new_stdio/high_level)

[More discussion at GNU libc](http://www.gnu.org/software/libtool/manual/libc/I_002fO-on-Streams.html#I_002fO-on-Streams)


[Low Level I/O (fd)](new_stdio/low_level)

[More discussion at GNU libc](http://www.gnu.org/software/libtool/manual/libc/Low_002dLevel-I_002fO.html#Low_002dLevel-I_002fO)

