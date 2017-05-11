# STRING FUNCTIONS (string.h)

 | Header     | [{z88dk}/include/string.h](https://raw.githubusercontent.com/z88dk/z88dk/master/include/string.h?view=markup)          |
 | -------------------------------------------------------------------------------------------------------------------------------------
 | Source     | [{z88dk}/libsrc/strings](https://github.com/z88dk/z88dk/tree/master/libsrc/strings/)                         |          
 | Include    | #include `<string.h>`                                                                                             |       
 | Linking    | n/a                                                                                                             |       
 | Compile    | n/a                                                                                                             |       
 | Comments   | These library functions are compiled as part of each target’s implicit library.                                 |     

The usual set of string functions specified by the ANSI standard and many additional ones besides.  Most text descriptions are reproduced from another source. ((Descriptions of ANSI functions taken from __The C Programming Language, Second Edition__ by Kernighan and Ritchie))

# String API

## void *memchr(void *s, uchar c, uint n)

Return a pointer to the first occurrence of character *c* in *s*, or 0 if not present among the first *n* characters.

## void *memrchr(void *s, uchar c, uint n)

Return a pointer to the last occurrence of character *c* in *s*, or 0 if not present among the first *n* characters.

## int memcmp(void *s1, void *s2, uint n)

Compare the first *n* characters of *s1* with *s2*; return as with *strcmp()*.

## void *memcpy(void *dst, void *src, uint n)

Copy *n* characters from *src* to *dst* and return *dst*.  Overlapped copy not guaranteed to work.

## void *memmove(void *dst, void *src, uint n)

Same as *memcpy()* except that it works even if the objects overlap.



## uchar *memopi(uchar *dst, uchar *src, uint n, uint op)

*** Uses IX ***

Non-standard function that copies *n* bytes from *src* to *dst* whilst performing an operation on the source and destination bytes.  *src* and *dst* are incremented as bytes are copied.

*op* can be:

*  0 = LOAD, straight copy of source to destination

*  1 = OR, bitwise or of destination and source bytes

*  2 = XOR, bitwise xor of destination and source bytes

*  3 = AND, bitwise and of destination and source bytes

*  4 = ADD, addition of destination and source bytes

*  5 = ADC, addition with carry of destination and source bytes

*  6 = SUB, subtraction of destination from source bytes

*  7 = SBC, subtraction with carry of destination from source bytes

*  8 = RLS, rotate left of source bytes

*  9 = RRS, rotate right of source bytes

If *op* is outside this range it is taken as the address of a C function that should be called to perform the operation.  The prototype of the called C function is:

	:::C
	uchar func(uchar dst_byte, uchar src_byte);


The C function should return the result of the operation applied to the source and destination bytes.

## uchar *memopd(uchar *dst, uchar *src, uint n, uint op)

*** Uses IX ***

As *memopi()* except *src* and *dst* are decremented as bytes are copied.

## void *memset(void *s, uchar c, uint n)

Place character *c* into the first *n* characters of *s* and return *s*.

## void *memswap(void *s1, void *s2, uint n)

Swap *n* bytes between *s1* and *s2* and return *s1*.  Overlapped swap is not defined except when s1==s2 in which case the swap occurs but the object is unchanged.

## char *strcat(char *dst, char *src)

Concatenate string *src* onto the end of string *dst*; return *dst*.

## char *strchr(char *s, uchar c)

Return pointer to first occurrence of *c* in *s* or 0 if not present.

## char *strchrnul(char *s, uchar c)

Return pointer to first occurrence of *c* in *s* or a pointer to a null string if not present.



## int strcmp(char *s1, char *s2)

Compare string *s1* to string *s2*; if *s1`<s2* return <0, if *s1==s2* return 0, if *s1>`s2* return >0.

## char *strcpy(char *dst, char *src)

Copy string *src* to string *dst* including '\0'; return *dst*.

## int strcspn(char *s1, char *s2)

Return length of the prefix of *s1* consisting of characters in *s2*.

## char *strdup(char *s)

Allocates sufficient memory for a copy of the string *s*, does the copy, and returns a pointer to it.  When finished with it, the returned string must be freed with free().  If the memory allocation is unsuccessful, 0 is returned.

//**NOTE:** strdup() is part of the malloc library.  To use it you must **#include `<malloc.h>` and link with **-lmalloc** during compilation.  z88dk also requires some initialization before using the malloc library.  See the [memory allocation page](http://www.z88dk.org/wiki/doku.php?id=library:memory_allocation#standard_c_malloc_library_malloc.h) for details.//

## int stricmp(char *s1, char *s2)

A caseless *strcmp()*.

## int strlcat(char *dst, char *src, uint size)

Appends string *src* to the end of string *dst*, stopping the append prematurely if the size of string *dst* reaches *size* bytes including the terminating NUL character.  The resulting string *dst* is always NUL terminated unless *size* is 0.  If *size* is less than the initial length of string *dst* then *dst* is not changed.

Always returns the sum of the initial length of *dst* and the initial length of *src*.  That is, the length of the resulting string had there been no buffer overrun concerns.

This is a 'safe' strcat() that guarantees the destination buffer will never be overrun and will always be terminated with a NUL character.

## int strlcpy(char *dst, char *src, int size)

Copies string *src* into string *dst*, writing at most *size* bytes and always terminating *dst* with a NUL character unless *size* is 0.  Always returns the length of the *src* string.

This is a 'safe' strcpy() that guarantees the destination buffer will never be overrun and will always be terminated with a NUL character.

## int strlen(char *s)

Return length of string *s*.

## char *strlwr(char *s)

Changes the characters in string *s* to lower case; return *s*.

## char *strncat(char *dst, char *src, uint n)

Concatenate at most *n* characters of string *src* to string *dst* and terminate *dst* with '\0'; return *dst*.

## int strncmp(char *s1, char *s2, uint n)

Compare at most *n* characters of string *s1* to string *s2*; if *s1`<s2* return <0, if *s1==s2* return 0, if *s1>`s2* return >0.


## char *strncpy(char *dst, char *src, uint n)

Copy at most *n* characters of string *src* to string *dst*; return *dst*.  Pad with '\0's if *dst* has fewer than *n* characters.  No '\0' is implicity appended to *dst* so *dst* will only have a terminating '\0' if the length of *src* is less than *n*.

## int strnicmp(char *s1, char *s2, uint n)

A caseless *strncmp()*.

## char *strpbrk(char *s, char *match)

Return pointer to first occurrence in string *s* of any character of string *match* or 0 if none are present.

## int strpos(char *s, uchar c)

Return the index of the first occurrence of character *c* in string *s*.  Searching for '\0' will return the length of the string.  If the character is not found, -1 is returned.

## char *strrchr(char *s, uchar c)

Return pointer to last occurrence of characer *c* in string *s* or 0 if not present.  Searching for '\0' will return a ptr to the end of the string.

## char *strrev(char *s)

Reverse the string *s*; return *s*.

## char *strrstr(char *s, char *w)

Return pointer to last occurrence of string *w* in string *s* or 0 if not present.

## char *strrstrip(char *s, uchar c)

Remove any occurrences of character *c* from the end of string *s*; return *s*.

## int strspn(char *s1, char *s2)

Return the length of the prefix of *s1* containing characters in *s2*.

## char *strstr(char *s, char *w)

Return a pointer to the first occcurrence of string *w* in string *s* or 0 if not present.

## char *strstrip(char *s, uchar c)

Remove any occurrences of the character *c* from the beginning of string *s*; return *s*.

## char *strtok(char *s, char *delim)

**//Contains static data, not ROMable without changes//**

*strtok()* splits string *s* into tokens delimited by characters from string *delim*.  The initial call to *strtok()* returns a pointer to the first token found in *s*.  Subsequent calls should be made with *s* equal to 0; in this case pointers to subsequent tokens found in the original *s* will be returned.  When all tokens have been returned, 0 will be the result.

*strtok()* writes '\0's into string *s* where delimiter characters are found.

## char *strsep(char **s, char *delim)

*strsep()* splits string pointed by *s* into tokens delimited by characters from string *delim*.  Returns a pointer to the first token found in *s* and sets the pointer to the end of the token.   When all tokens have been returned, 0 will be the result.

*strsep()* writes '\0's into string *s* where delimiter characters are found.

## char *strupr(char *s)

Chnages the characters in *s* to upper case; return *s*.

# Old C compatibility stuff

## void *strset(void *s, uchar c)

Place character *c* repeated in all *s* and return *s*.

## int strcmpi(char *s1, char *s2)

A caseless *strcmp()*, alias of stricmp.


## int strncmpi(char *s1, char *s2, uint n)

A caseless *strncmp()*, alias of strnicmp.

## void *rawmemchr(void *s, uchar c)

Return a pointer to the first occurrence of character *c* in *s*.
Occurence must exist.


# BSD compatibility stuff

## int bcmp(void *s1, void *s2, uint n)

*Implemented as a macro to memcmp()*

Returns 0 if the first *n* bytes of *s1* are identical to the first *n* bytes of *s2*.

## void bcopy(void *src, void *dst, uint n)

*Implemented as a macro to memcpy()*

Copy the first *n* bytes of *src* to *dst*.  Overlapped copy not guaranteed to work.

## void bzero(void *s, uint n)

*Implemented as a macro to memset()*

Write zero into the first *n* bytes of *s*.

## char *index(char *s, uchar c)

*Implemented as a macro to strchr()*

Return a pointer to the first occurrence of character *c* in string *s* or 0 if not found.

## char *rindex(char *s, uchar c)

*Implemented as a macro to strrchr()*

Return a pointer to the last occurrence of character *c* in string *s* or 0 if not found.

## int strcasecmp(char *s1, char *s2)

// Implemented as a macro to stricmp()//

An alias for *stricmp()*.

## int strncasecmp(char *s1, char *s2, uint n)

*Implemented as a macro to strnicmp()*

An alias for *strnicmp()*.

