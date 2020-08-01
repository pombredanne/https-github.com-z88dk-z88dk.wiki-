# Breaking the 64k barrier

The Z80 processor is an old processor, and it's based on an even older one - the Intel 8080. These two processors are 8 bit accumulator based and can access an address space of 65536 bytes (or 64k).

Quite early on it was realised that this simply isn't enough address space to implement a comprehensive operating system and leave sufficient user RAM for their own programs.

The solution to this is quite straightforward, have more ROM/RAM in the system and page it in when required. This trick isn't just used on the Z80 but also for the 6500 processors.

# Named address spaces (sccz80 and zsdcc)

Both sccz80 and sdcc support a subset of the Embedded C standard named address spaces. Both of them use them for implementing bank switching. Some examples follow:

```
void setb0(void);            // The function that sets the currently active memory bank to b0
void setb1(void);            // The function that sets the currently active memory bank to b1
__addressmod setb0 spaceb0;  // Declare a named address space called spaceb0 that uses setb0
__addressmod setb1 spaceb1;  // Declare a named address space called spaceb1 that uses setb1

spaceb0 int x;          // An int in address space spaceb0
spaceb1 int ∗y;         // A pointer to an int in address space spaceb1
spaceb0 int ∗spaceb1 z; // A pointer in address space spaceb1 that points to an int in address space spaceb0
```

# Far Memory (sccz80)

## Abstracted Handling

z88dk supports the concept of a `__far` pointer. This a type qualifier that can be prefixed to any pointer type, and as far as the programmer is concerned, handling of them should be transparent.

The far pointers used by z88dk are 24 bits long i.e. 3 bytes, this enables the theoretical access of 16Mb of memory. The far pointer allows for a *flat* memory model - i.e. they are treated as "normal" 24 bit number by the compiler. Far pointers are always handled in ''ehl'' where e contains bits 16-23 and hl bits 0-15 of the pointer. If e holds 0 then hl is taken to be an absolute address in the current memory configuration.

## Compiler support

Out of the two compilers with z88dk, only sccz80 supports far pointers, at present there is just a single implementation of far support - for the z88.

A far pointer can be declared using the left-associative `__far` decorator, for example:

    char *__far string;
    void (*__far function_pointer)(int param);

## Implementing Far memory

Implementing of support for far memory is of course machine specific and depends on the target machines memory model. Z88dk makes certain assumptions that routines are available, in order to have a functioning far memory system for your target the functions detailed in this file will need to be implemented.

### stdlib functionality

	 void *__far malloc_far(long num_bytes)


This function should reserve ''num_bytes'' space in the system, and return a value in ehl which provides a handle to the allocated memory.

Correspondingly the following functions are also required:

	 void *__far calloc_far(int count, int size);
	void free_far(void *__far ptr);

The following function might be useful to deallocate all allocated memory by an application, this would normally be called by the [[porting::startupcode|startup] code just before exiting.

	void freeall_far()


*The _far suffix is used to distinguish between near and far allocation methods.*

### "crt" functions

sccz80 will generate various calls to functions whenever data is required to be accessed from a far location, these are used to pick up the various primitive datatypes from far memory (it has no knowledge of the actual implementation of far memory).

#### Reading routines

These have entry ehl = far address address:

	lp_gchar   - read char
	Exit:   a=l = byte at that location
	        h=0
	
	lp_gint    - read 2 byte integer
	Exit:   hl = word at that location (stored little endian)
	
	lp_gptr
	Exit:  ehl = far pointer at that location
	
	lp_glong        - read 4 byte integer
	Exit: dehl = long at that location (l=LSB)
	
	lp_gdoub        - read 6 byte double
	
	Exit: Load FA -> FA+5 with the double stored at that location


For ''lp_gchar'', the function should not worry about signedness, the compiler will resolve that.

FA -> FA+5 are 6 bytes presently located in the crt0 file.

#### Writing routines

These have entry e'h'l' = far address and the following additional entry parameters:

	lp_pchar        - write char
	Entry:     l = byte to write
	
	lp_pint         - write 2 byte integer
	Entry:    hl = word to write
	
	lp_pptr         - write far pointer
	Entry:   ehl = far pointer to write
	
	lp_plong        - write 4 byte integer
	Entry:  dehl = long to write
	
	lp_pdoub        - write double
	Entry:  none


''lp_pdoub'' should write the bytes stored in FA -> FA+5 to the far memory address.

#### Far pointer calling

In order to support calling routines located in banked memory, you will need to implement the `l_farcall`support function. This takes ehl as the far address to call. 

### String Routines

You should also implement the following string.h functions which will make the usage of far memory a lot more convenient:

	
	extern int strlen_far(far char *);
	extern far char *strcat_far(far char *, far char *);
	extern far char *strcpy_far(far char *, far char *);
	extern far char *strncat_far(far char *, far char *, int);
	extern far char *strncpy_far(far char *, far char *, int);
	extern far char *strlwr_far(far char *);
	extern far char *strupr_far(far char *);
	extern far char *strchr_far(far unsigned char *, unsigned char);
	extern far char *strrchr_far(far unsigned char *, unsigned char);
	extern far char *strdup_far(far char *);

# __banked function calls (sccz80 and zsdcc)


# Trampoline function calls (sccz80 and zsdcc)

_To be written_ See #641

