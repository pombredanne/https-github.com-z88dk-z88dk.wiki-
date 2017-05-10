# STANDARD LIBRARY (stdlib.h)

 | Header     | [{z88dk}/include/stdlib.h](https///raw.githubusercontent.com/z88dk/z88dk/master/include/stdlib.h)          |       
 | -------------------------------------------------------------------------------------------------------------------------       
 | Source     | [{z88dk}/libsrc/stdlib](https///github.com/z88dk/z88dk/tree/master/libsrc/stdlib/)                           |     
 | Include    | #include `<stdlib.h>`                                                                                             |  
 | Linking    | n/a                                                                                                             |  
 | Compile    | n/a                                                                                                             |  
 | Comments   | These library functions are compiled as part of each target’s implicit library.                                 |

These are utility functions defined as part of the C standard.  Some functions have been modified to better suit the z80 and some functions have been added that are useful specifically on z80 machines.  The set of standard functions is nearly complete, with the float/double to string conversions missing.

# General

Most text reproduced here from another source.((Descriptions for standard functions taken from __The C Programming Language, 2nd Edition__ by Kernighan and Ritchie))

## int atoi(char *s)

Converts *s* into an integer.

## int itoa(int n, char *s, unsigned char radix)

Converts the integer value *n* to a null-terminated string using the specified base *radix* and stores the result in the array given by *s*.  If *n* is less than zero, the resulting string is preceeded by a minus sign.  Returns the number of characters written to *s* not including the terminating NUL char.

If *s* is NULL, the string is not written.  However a count of the number of characters that would have been written is still returned.

## int utoa(unsigned int n, char *s, unsigned char radix)

As itoa() above but the number *n* is unsigned.

## long atol(char *s)

Converts *s* into a long.

## int ltoa(long n, char *s, unsigned char radix)

***Uses EXX, IX***

As itoa() above but the number *n* is a long.

## int ultoa(unsigned long n, char *s, unsigned char radix)

***Uses EXX, IX***

As itoa() above but the number *n* is an unsigned long.

## long strtol(char *s, char **endp, int base)

**//Uses AF', EXX, IX//**

*strtol* converts the prefix of *s* to long, ignoring leading white space; it stores a pointer to any uncoverted suffix in **endp* unless *endp* is 0.  If *base* is between 2 and 36, conversion is done assuming that the input is written in that base.  If *base* is zero, the base is 8, 10, or 16; leading 0 implies octal and leading 0x or 0X hexadecimal.  Letters in either case represent digits from 10 to *base*-1; a leading 0x or 0X is permitted in base 16.  No overflow is detected.

## unsigned long strtoul(char *s, char **endp, int base)

**//Uses AF', EXX, IX//**

*strtoul* is the same as *strtol* except that the result is unsigned long.  No overflow is detected.

## int rand(void)

*rand* returns a pseudo-random integer in the range 0 to *RAND_MAX*, which is at least 32767.  A seed variable is supplied by the startup code that can be replaced on some platforms by declaring an "int std_seed" (or equivalent) somewhere within your project and using the pragma #pragma output HAVESEED

## void srand(unsigned int seed)

*srand* seeds the seed for a new sequence of pseudo-random numbers.  The seed variable is transparently supplied by the platform's startup code but can be replaced on some platforms as discussed under rand() above.

## void *calloc(int nobj, int size)

*calloc* returns a pointer to space for an array of *nobj* objects, each of size *size*, or 0 if the request cannot be satisfied.  The space is initialized to zero bytes.  z88dk requires the program to initialize the heap prior to its use.  See the [malloc](library/memory_allocation#standard_c_malloc_library_malloc.h) library description for more details.  In addition some targets (currently the z88) can support a larger than 64k memory model using the [far memory model](advanced/farmem).

## void *malloc(int size)

*malloc* returns a pointer to space for an object of size *size*, or 0 if the request cannot be satisfied.  The space is uninitialized.  z88dk requires the program to initialize the heap prior to its use.  See the [malloc](library/memory_allocation#standard_c_malloc_library_malloc.h) library description for more details.  In addition some targets (currently the z88) can support a larger than 64k memory model using the [far memory model](advanced/farmem).

## void *realloc(void *p, int size)

*realloc* changes the size of the object pointed to by *p*; it does nothing if *p* is 0.  *p* must be a pointer to space previously allocated by *calloc*, *malloc* or *realloc*.  z88dk requires the program to initialize the heap prior to its use. See the [malloc](library/memory_allocation#standard_c_malloc_library_malloc.h) library description for more details.  In addition some targets (currently the z88) can support a larger than 64k memory model using the [far memory model](advanced/farmem). 

## void free(void *p)

*free* deallocates the space pointed to by *p*; it does nothing if *p* is 0.  *p* must be a pointer to space previously allocated by *calloc*, *malloc* or *realloc*.  See the [malloc](library/memory_allocation#standard_c_malloc_library_malloc.h) library description for more details.  In addition some targets (currently the z88) can support a larger than 64k memory model using the [far memory model](advanced/farmem).

## void abort(void)

Equivalent to *exit(15)*

## void exit(int status)

*exit* causes normal program termination.  *atexit* functions are called in reverse order of registration, open files are flushed, open streams are closed, and control is returned to the environment.  How *status* is returned to the environment is implementation dependent, but zero is taken as successful termination.  The values *EXIT_SUCCESS* and *EXIT_FAILURE* may also be used.

## int atexit(void (*fcn)(void))

*atexit* registers the function *fcn* to be called when the program terminates normally; it returns non-zero if the registration cannot be made.

## void sleep(int)

Wait for a period of time measured in seconds.


## void csleep(int)

Wait for a period of time measured in centiseconds.


## void delay(int)

Wait for a period of time measured in milliseconds.


## void t_delay(int)

Accurate CPU T_STATE delay (min 141 T)



## int getopt(int nargc, char **nargv, const char *ostr)

Not ROMable.

## void l_bsearch(void *key, void *base, unsigned int n, char (*cmp)(void *keyval, void *datum))

**//Uses IX, IY//**

*l_bsearch* performs a [binary search](http://en.wikipedia.org/wiki/Binary_search) on //base[0]...base[n-1]// for an item that matches **key*.  The function *cmp* must return negative if its first argument (the search key) is less than its second (a table entry), zero if equal, and positive if greater.  Items in the array *base* must be in ascending order.  *bsearch* returns a pointer to the matching item or 0 if none exists.

*l_bsearch* differs from the standard library's *bsearch* in the assumption that all items in the *base* array are two bytes in size.  This means you can search an array of integers, two-byte chars or pointers to objects.  The general case specified by the library would negatively impact on z80 performance.

## void l_qsort(void *base, size_t n, char (*cmp)(void *keyval, void *datum))

**//Uses AF', IX, IY//**

*l_qsort* sorts into ascending order an array //base[0]...base[n-1]// of objects.  The comparison function *cmp* is as in *bsearch*.

*l_qsort* differs from the standard library's *qsort* in the assumption that all items in the *base* array are two bytes in size.  This means you can sort an array of integers, two-byte chars or pointers to objects.  The general case specified by the library would negatively impact on z80 performance.

Although this implementation of [quicksort](http://en.wikipedia.org/wiki/Quicksort) is the so-called iterative version, it can still require stack space proportional to the number of elements being sorted in degenerative cases.  For this reason, on small 8-bit machines like the z80, it is usually preferable to use a heapsort which is similarly quick but does not require any additional memory to perform the sort.  You can find a heapsort implementation in the [abstract data types](library/adt#heap_api) library described under the *adt_HeapExtract()* function.

## int abs(int n)

Returns the absolute value of its integer argument.

## long labs(long n)

Returns the absolute value of its long argument.


# Non-Standard Additions

## unsigned char inp(unsigned int port)

Reads a 16-bit i/o port and returns the result.  The header includes a macro version M_INP(PORT) that will generate inline assembler for the same purpose.  The PORT argument in the macro must resolve to a constant so the macro cannot act as a replacement for the function call in all circumstances.

## void outp(unsigned int port, unsigned char byte)

Writes the byte to the 16-bit i/o port.  The header includes a macro version M_OUTP(PORT,BYTE) that will generate inline assembler for the same purpose.  Both arguments to the macro must resolve to constants so the macro cannot act as a replacement for the function call in all circumstances.

## int sleep(int seconds)

Wait for a period of time measured in seconds.

## int csleep(int centiseconds)

Wait for a period of time measured in hundredths of seconds.

## int delay(int milliseconds)

Wait in a similar way to the well known fuction implemented on other compilers.

## unsigned long extract_bits(unsigned char *data, unsigned int start, unsigned int size) 

Extract a given number of bits from a byte string (at specified bit position) into a long value

## void t_delay(unsigned int tstates)

An accurate delay measured in z80 T-states (cycles).  *tstates* must be at least 141.  The delay time includes the CALL to and RET from the function but does not include any time required by the compiler to collect the *tstates* parameter.  If *tstates* is a constant this extra time would be 10 cycles.

## void *swapendian(void *addr)

Returns the result of swapping the most significant and least significant bytes in the address passed as parameter.

## void  bpoke(void *addr, unsigned char byte)

Write the byte into the address specified.  The header includes a macro version M_BPOKE(ADDR,BYTE) that will generate inline assembler for the same purpose.  Both arguments to the macro must resolve to constants so the macro cannot act as a replacement for the function call in all circumstances.

## void  wpoke(void *addr, unsigned int word)

Write the word to the address specified.  The word will be written in the usual z80 little endian way.  The header includes a macro version M_WPOKE(ADDR,WORD) that will generate inline assembler for the same purpose.  Both arguments to the macro must resolve to constants so the macro cannot act as a replacement for the function call in all circumstances.

## unsigned char bpeek(void *addr)

Reads the byte stored at the address specified and returns the result.  The header includes a macro version M_BPEEK(ADDR) that will generate inline assembler for the same purpose.  The macro argument must resolve to a constant so the macro cannot act as a replacement for the function call in all circumstances.

## unsigned int wpeek(void *addr)

Reads the little endian word stored at the address specified and returns the result.  The header includes a macro version M_WPEEK(ADDR) that will generate inline assembler for the same purpose.  The macro argument must resolve to a constant so the macro cannot act as a replacement for the function call in all circumstances.

## unsigned int isqrt(unsigned int n)

Return the integer square root of *n*. Where *n* is less than 16384.



# Z88 Only

## int system(char *s)

Executes a CLI command, details of the format of the supplied string can be found at  http://www.worldofspectrum.org/z88forever/dn327/stdiocli.htm


