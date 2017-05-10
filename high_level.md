
# STANDARD I/O HIGH LEVEL API

| Header     | not available yet                                                                                               |
| -----------|-----------------------------------------------------------------------------------------------------------------|
| Source     | [{z88dk}/libsrc/stdio_new](https///github.com/z88dk/z88dk/tree/master/libsrc/stdio_new/)                     |
| Include    | #include `<stdio.h>`                                                                                              |
| Linking    | n/a                                                                                                             |
| Compile    | n/a                                                                                                             |
| Comments   | These library functions are compiled as part of each target’s implicit library.                                 |

Some of the text descriptions below are taken from __The C Programming Language, 2nd Editon__ (Kernighan and Ritchie) and the [GNU Portability Library Manual](http://www.gnu.org/software/gnulib/manual/).

# File Operations

## FILE *fopen(char *filename, char *mode)

Opens the named file returning a stream or 0 if the open fails.

Legal values for *mode* include:


*  "r" open for reading, file must exist

*  "w" open for writing, file created and/or truncated

*  "a" open for writing, writes append to file, file created if necessary

*  "r+" open for reading and writing, file must exist

*  "w+" open for reading and writing, file created and/or truncated

*  "a+" open for reading and writing, writes append to file, file created if necessary

In z88dk the filename contains a device driver selector in the first two characters as in "m>....".  If a single letter device identifier is found ('m' in this example) the open request will be passed to that device.  If no device identifier is present, the open request goes to the default device driver.  The platform's crt0 contains letter / device associations in a table where device drivers can be added or removed from a project by the user.  Likewise the crt0 also identifies the default device driver.  The contents of the filename following the optional device identifier are interpretted by the device driver.  A disk device may interpret the remaining filename as a path to a file whereas a serial driver may interpret the rest of the filename as specification of baud rate, parity, etc.

If an error occurs, 0 will be returned with the carry flag set and *errno* will be set to one of:


*  [EDEVNF] the indicated device driver was not found

*  [EINVAL] the mode string is invalid

*  [ENOMEM] insufficient memory to create a new FILE struct

The device driver may set *errno* to other values.

## FILE *freopen(char *filename, char *mode, FILE *stream)

Closes *stream* and then opens a new file as in *fopen()*, reusing the same FILE structure.  *freopen()* is normally used to change the files associated with *stdin*, *stdout*, *stderr*.  Returns *stream*.

If an error occurs, 0 will be returned with the carry flag set, *stream* will be closed and *errno* will be set to:


*  [EDEVNF] the indicated device driver was not found

*  [EINVAL] the mode string is invalid

The device driver may set *errno* to other values.



## int fflush(FILE *stream)

On an output stream causes any buffered data to be sent to the device driver.  On an input stream the effect is undefined, though under z88dk the minimum action is to reposition the file pointer in the device driver by the number of unread bytes sitting in high level buffers and to clear those high level buffers.  In streams supporting both input and output, the input is flushed first and then the output.  Returns 0 to indicate success.

If an error occurs, -1 will be returned with the carry flag set and *errno* set by the device driver.

*fflush(NULL)* causes all open FILEs to be flushed.

## int fclose(FILE *stream)

Flush any unwritten data to the device driver, discard any unread buffered input, free any allocated buffers, and close *stream*.  Returns 0 to indicate success.

If an error occurs, -1 will be returned with carry flag set and *errno* set by the device driver.

# Low Level File Descriptor

## FILE *fdopen(int fd, const char *mode)

Associate a stream with the low-level file descriptor.  Closing the returned FILE will also close the underlying fd.

*mode* is a character string indicating how the file will be used.  Its meaning is exactly as specified in fopen() except that modes containing "w" will not cause file truncation.

If an error occurs, 0 will be returned with the carry flag set and *errno* will be set to one of:


*  [EBADF] fd is out of range or there is no open file with descriptor fd

*  [EINVAL] the mode string is not understood or the underlying fd does not support the mode

*  [ENOMEM] insufficient memory to create a new FILE struct

## int fileno(FILE *stream)

Return the integer file descriptor associated with *stream*.

If an error occurs, -1 will be returned with the carry flag set and *errno* will be set to one of:


*  [EBADF] stream is not valid

# Formatted Output


## int fprintf(FILE *stream, char *format, ...)

Writes formatted output to *stream* under the control of the *format* string using data from arguments following *format*.  Returns the number of characters written or -1 if an error occurs.

The format string contains two types of objects: ordinary characters, which are copied to the output stream, and conversion specifiers, each of which causes the next argument of *fprintf* to be output to the stream as specified.  Each conversion specifier begins with % and ends with a conversion character.  Between the % and the conversion character there may be optional modifiers, in order:


*  flags (any order) which modify the conversion:
    * -, specifies left adjustment of the converted argument in its field
    * +, specifies that the number will always be printed with a sign, plus or minus
    * *space*, specifies that if a positive number is to be printed with a sign, use a space rather than +
    * 0, for numbers specifies padding to the field width with leading zeroes
    * #, for numbers specifies that a base prefix should be added ('%' for binary, '0' for octal, '0x' for hexadecimal)


*  width, a number specifying a minimum field width.  The conversion argument will be printed in a field at least this wide and wider if necessary.  If padding is necessary, spaces will be added unless the zero flag is present for numbers in which case 0s will be used.  z88dk limits the specified field width to 255 characters.


*  a period which separates the field width from the precision


*  precision, a number that specifies the maximum number of characters to be printed from a string or the minimum number of digits to be printed for a number (leading 0s will be added to make up the necessary width).  z88dk limits precision for numbers to 32 characters unless this is changed at library compile time.  z88dk limits the specified precision to 255 characters in all cases.

Width or precision may be specified as "*" in which case the value used is read from the argument list.

The conversion characters and their meanings are shown below.  If the character after % is not listed below, "%?" is output on the stream where "?" is the unrecognized conversion character.

 | CHARACTER | ARGUMENT TYPE | CONVERTED TO                                                        | 
 | --------- | ------------- | ------------                                                        | 
 | d,i       | 16-bit int    | signed decimal                                                      | 
 | b         | 16-bit int    | unsigned binary                                                     | 
 | o         | 16-bit int    | unsigned octal                                                      | 
 | x,X       | 16-bit int    | unsigned hexadecimal, X uses capitals                               | 
 | u         | 16-bit int    | unsigned decimal                                                    | 
 | ld,li     | 32-bit long   | signed decimal                                                      | 
 | lb        | 32-bit long   | unsigned binary                                                     | 
 | lo        | 32-bit long   | unsigned octal                                                      | 
 | lx,lX     | 32-bit long   | unsigned hexadecimal, X uses capitals                               | 
 | lu        | 32-bit long   | unsigned decimal                                                    | 
 | c         | char          | single character                                                    | 
 | s         | char *        | string of characters up to \0 or until precision exhausted          | 
 | p,P       | void *        | pointer (printed as x,X with leading 0x)                            | 
 | lp,lP     | 24-bit void * | pointer (printed as lx,lX with leading 0x)                          | 
 | n         | int *         | number of characters output so far is *written* into the argument | 
 | %         | none          | output a %                                                          | 

**NOTE:** Some or all of these converters may be omitted by the user at program compile time.  Floating point has not yet been added.

If an error occurs, -1 is returned with the carry flag set and *errno* will be set to:


*  [EACCES] the stream is not open for output

The device driver may set errno to other values.

## int printf(char *format, ...)

Equivalent to *fprintf(stdout, char *format, ...)*.

## int sprintf(char *s, char *format, ...)

*sprintf()* is the same as *printf()* except that the output is written into string *s*, terminated with '\0'.  *s* must be big enough to hold the result.  The return count does not include the '\0'.

## int snprintf(char *s, unsigned int size, char *format, ...)

As *sprintf()* but limits the number of bytes written to the string *s* to *size* bytes.  This size includes space for the terminating '\0' character.  The return value is always the length of the string that would have been written without size restriction, not including the terminating '\0' byte.

With *size* set at zero the program can determine how long the resulting string will be without writing any characters to the buffer.


## int asprintf(char **s, char *format, ...)

As *sprintf()* but dynamically allocates a buffer large enough to hold the resulting string using *malloc()*.  The allocated buffer is written to *s* prior to exit.  It is the program's responsibility to *free* the allocated buffer.

If insufficient memory is available for the allocation *s* is set to 0, -1 is returned with carry flag set and *errno* will be set to:


*  [ENOMEM] insufficient memory available to dynamically allocate a buffer

## Related Functions

Also present: vfprintf(), vprintf(), vsprintf(), vsnprintf(), vasprintf()


# Formatted Input





## int fscanf(FILE *stream, char *format, ...)

Reads from *stream* under the control of the *format* string and assigns converted values to subsequent arguments, **each of which must be a pointer**.  It returns when *format* is exhausted or an error occurs.  *fscanf* returns -1 if no characters could be read from the stream.  Otherwise it returns the number of conversions performed *and* assigned.

The *format* string usually contains conversion specifications which are used to direct interpretation of input.  The *format* string may contain:


*  Blanks, tabs or other whitespace which cause any leading whitespace to be consumed from *stream*

*  Ordinary characters (not %) which are expected to match the subsequent characters read from *stream*

*  Conversion specifications consisting of a %, an optional assignment suppression character *, an optional number indicating a maximum field width, and a conversion character.

A conversion specification determines how the next bytes on the stream are interpretted.  Normally the result is placed in the variable pointed to by the next argument in the argument list.  If assignment suppression is indicated by *, as in %*s, the input field is read from the stream but no assignment is made.  An input field is defined as a string of non-whitespace characters; it extends until the next whitespace character or until the field width, if specified, is exhausted (z88dk's implementation ignores the field width when converting numbers).  This implies that *scanf* will read across line boundaries to find its input since newlines are considered whitespace.  

The conversion character indicates how the input field is interpretted.  The conversion characters are listed below:

 | CHARACTER     | INPUT DATA                       | ASSIGNED TO        | 
 | ---------     | ----------                       | -----------        | 
 | d             | decimal integer                  | int *              | 
 | i             | integer `<sup>`1`</sup>`             | int *              | 
 | b             | binary integer `<sup>`7`</sup>`      | int *              | 
 | o             | octal integer `<sup>`2`</sup>`       | int *              | 
 | u             | unsigned decimal integer         | unsigned int *     | 
 | x             | hexadecimal integer `<sup>`3`</sup>` | int *              | 
 | ld            | decimal long                     | long *             | 
 | li            | long `<sup>`1`</sup>`                | long *             | 
 | lb            | binary long `<sup>`7`</sup>`         | long *             | 
 | lo            | octal long `<sup>`2`</sup>`          | long *             | 
 | lu            | unsigned decimal long            | unsigned long *    | 
 | lx            | hexadecimal long `<sup>`3`</sup>`    | long *             | 
 | c             | characters `<sup>`4`</sup>`          | char *             | 
 | s             | characters `<sup>`8`</sup>`          | char *             | 
 | p             | see x                            | int *              | 
 | lp            | see lx                           | long *             | 
 | n             | num of chars read thus far       | int *              | 
 | [..]          | characters `<sup>`5`</sup>`          | char *             | 
 | [//carat//..] | characters `<sup>`6`</sup>`          | char *             | 
 | %             | match % from *stream*          | no assignment made | 

`<sup>`1`</sup>` may be in octal (leading 0), decimal or hexadecimal (leading 0x)
`<sup>`2`</sup>` with or without leading 0
`<sup>`3`</sup>` with or without leading 0x
`<sup>`4`</sup>` chars are written to the array up to the width (default=1) specified, whitespace is not skipped
`<sup>`5`</sup>` matches longest non-empty string of chars from *stream* in the set, terminating '\0' is added
`<sup>`6`</sup>` matches longest non-empty string of chars from *stream* not in the set, terminating '\0' is added
`<sup>`7`</sup>` with or without leading %
`<sup>`8`</sup>` leading whitespace is skipped then characters are read until the next whitespace char

**NOTE:** Some or all of these converters may be omitted by the user at program compile time.  Floating point has not yet been added.

If an error occurs, the carry flag is set and *errno* will be set to:


*  [EACCES] the stream is not open for input

The device driver may set other errors.  *errno* is not set if there is a character mismatch or a conversion could not be performed because of invalid input on the stream.

## int scanf(char *format, ...)

Equivalent to *fscanf(stdion, format, ...)*.

## int sscanf(char *s, char *format, ...)

As *scanf()* except input characters are read from string *s* rather than a stream.

## Related Functions

Also present: vfscanf(), vscanf(), vsscanf()


# Character Input and Output

## int getc(FILE *stream)

## char *fgets(char *s, int n, FILE *stream)

## int fputc(int c, FILE *stream)

## int fputs(char *s, FILE *stream)

## int getc(FILE *stream)

## int getchar(void)

Equivalent to *getc(stdin)*.

## char *gets(char *s)

## int putc(int c, FILE *stream)

## int putchar(int c)

## int puts(char *s)

## int ungetc(int c, FILE *stream)

## int getline(char **lineptr, int *n, FILE *stream)

## int getdelim(char **lineptr, int *n, int delim, FILE *stream)


# Direct Input and Output

## int fread(void *ptr, int size, int nobj, FILE *stream)

## int fwrite(void *ptr, int size, int nobj, FILE *stream)


# File Positioning

## int fseek(FILE *stream, long offset, int whence)

## long ftell(FILE *stream)

## void rewind(FILE *stream)

## int fgetpos(FILE *stream, fpos_t *ptr)

## int fsetpos(FILE *stream, fpos_t *ptr)


# Error Functions

## int feof(FILE *stream)

## int ferror(FILE *stream)

