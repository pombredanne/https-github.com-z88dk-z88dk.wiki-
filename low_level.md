
# STANDARD I/O LOW LEVEL API

 | Header     | not available yet                                                                                               |  
 | ------------------------------------------------------------------------------------------------------------------------------  
 | Source     | [{z88dk}/libsrc/stdio_new/fd](https://github.com/z88dk/z88dk/tree/master/libsrc/stdio_new/fd/)               |     
 | Include    | #include `<stdio.h>`                                                                                              |  
 | Linking    | n/a                                                                                                             |  
 | Compile    | n/a                                                                                                             |  
 | Comments   | These library functions are compiled as part of each target’s implicit library.                                 |

Some of the text descriptions below are taken from __The C Programming Language, 2nd Editon__ (Kernighan and Ritchie) and the [GNU Portability Library Manual](http://www.gnu.org/software/gnulib/manual/).

# File Descriptor Operations

## int open(char *filename, int flags)

## int dup(int fd)

## int dup2(int fdsrc, int fddst)

## int filter(?)

not implemented yet

## int pipe(int fd[2])

not implemented yet

## int close(int fd)

## void closeall(void)


# File Positioning

## long lseek_callee(int fd, long offset, int whence)


# Direct Input and Output

## int readbyte(int fd)

## int writebyte(int fd, int c)

## int read(int fd, void *dst, int size)

## int write(int fd, void *src, int size)


# Device Driver

## int fsync(int fd)

## int ioctl(int fd, int request, int arg)

## int poll(struct pollfd fds[], uint nfds, uint timeout=0)

## int pollfd(int fd, char event)

## void *setdefdev(void *driver_function)

