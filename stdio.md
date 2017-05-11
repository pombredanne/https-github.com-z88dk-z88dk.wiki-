 ====== STANDARD I/O (stdio.h) ======

 | Header     | [{z88dk}/include/stdio.h](https://raw.githubusercontent.com/z88dk/z88dk/master/include/stdio.h)            |       
 | -------------------------------------------------------------------------------------------------------------------------       
 | Source     | [{z88dk}/libsrc/stdio](https://github.com/z88dk/z88dk/tree/master/libsrc/stdio/)                             |     
 | Include    | #include `<stdio.h>`                                                                                              |  
 | Linking    | n/a                                                                                                             |  
 | Compile    | n/a                                                                                                             |  
 | Comments   | These library functions are compiled as part of each target’s implicit library.                                 |

These are IO functions defined as part of the C standard with some additions to suit development on Z80 based machines.

# General

Most text reproduced here from another source.((Descriptions for standard functions taken from __The C Programming Language, 2nd Edition__ by Kernighan and Ritchie))

# File Operations

## FILE *fopen(char *name, unsigned char *mode)

''fopen'' opens the named file, and returns a stream, or NULL if the attempt fails. Legal vales for ''mode'' are:

   * "r"  open a file for reading
   * "w"  open a file for writing
   * "a"  append, open or create a file for writing at the end

Not all targets support the "a" operation, depending on the implementation, files will may be truncated or ''fopen'' may return NULL.

## FILE *freopen(char *name, unsigned char *mode, FILE *fp)

''freopen'' opens the file with the specified mode and associats the stream with it. It returns *fp* or NULL if an error occurs.

## int fflush(FILE *)

This function exists purely as a stub in z88dk.

## int fclose(FILE *fp)

''fclose'' closes the stream ''fp''.

## extern int remove(char *name)

''remove'' removes the named file. It may not be supported for all targets.

## int rename(char *s, char *d)

''rename'' renames file ''s'' so that is has the filename ''d''. It may not be supported for all targets.

# Character Input and Output Functions

## int fgetc(FILE *fp)

''fgetc'' returns the next character of ''fp'' as an unsigned char, or ''EOF'' if end of file or error occurs.

## char *fgets(unsigned char *s, int n, FILE *fp)

''fgets'' reads at most the next ''n-1'' characters into the array ''s'', stopping if a newline is encountered; the new line is included in the array which is nul terminated. ''fgets'' returns ''s'', or ''NULL'' if end of file or an error occurs.

## int fputc(int c, FILE *fp)

''fputc'' writes the character c on stream ''fp''. It returns the character written or ''EOF'' for error.

## int fputs(unsigned char *s,  FILE *fp)

''fputs'' writes the stream s on stream ''fp''. It returns non-negative, or ''EOF'' for error.

## int gets(char *s)

''gets'' reads the input line into array ''s''. This function should not be used.

## int puts(unsigned char *s)

''puts'' writes the string ''s'' and a newline to the stdout.

## int ungetc(int c, FILE *fp)

''ungetc'' pushs c back onto the stream ''fp'' where it will be returned on the next read. Only one character of pushback per stream is implemented by z88dk.

# Direct Input and Output Functions

## int fread(void *ptr, int size, int num, FILE *fp)

''fread'' reads from stream ''fp'' into the array ''ptr'' at most ''num'' objects of size ''size'' returning the number of objects read.

## int fwrite(void *ptr, int size, int num, FILE *fp)

''fwrite'' write to stream ''fp'' from the array ''ptr'', ''num'' objects of size ''size'' returning the number of objects read.

# File Positioning Functions

## int fseek(FILE *fp, fpos_t offset, int whence)

## fpos_t ftell(FILE *fp)

## int fgetpos(FILE *fp, fpos_t *pos)

## int feof(FILE *fp)

# Formatted Input and Output Functions

## int printf(char *,...)

## int fprintf(FILE *,char *,...)
## int sprintf(char *,char *,...)

## int vsprintf(char *str,unsigned char *fmt,void *ap)
## int scanf(unsigned char *fmt,...)

## int fscanf(FILE *,unsigned char *fmt,...)
## int sscanf(char *,unsigned char *fmt,...)

## int vfscanf(FILE *, unsigned char *fmt, void *ap)
## int vsscanf(char *str, unsigned char *fmt, void *ap)

# Streamlined Console Functions

## int fgetc_cons()

## int fputc_cons(char c)
## int fgets_cons(char *s, int n)

## void puts_cons()
## void printk(char *fmt,...)

## int getk()

# Non-standard functions

## printn(int number, int radix,FILE *file)

## int ltoa_any(long in,unsigned  char *str, int sz, unsigned int radix, int signflag)


##  FILE *fdopen(int fildes, unsigned char *mode) 

## long fdtell(int fd)
## int fdgetpos(int fd, fpos_t *posn)



## void closeall()

## void fabandon(FILE *)


# Internal Functions

## int getarg(void)

## int fchkstd(FILE *)
