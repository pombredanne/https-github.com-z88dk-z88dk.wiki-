Implementing of support for far memory is of course machine specific and depends on the target machines memory model. Z88dk makes certain assumptions that routines are available, in order to have a functioning far memory system for your target the functions detailed in this file will need to be implemented.

# stdlib functionality

	far void *malloc_far(long num_bytes)


This function should reserve ''num_bytes'' space in the system, and return a value in ehl which provides a handle to the allocated memory.

Correspondingly the following functions are also required:

	far void *calloc_far(int count, int size);
	void free_far(far void *ptr);


The following function might be useful to deallocate all allocated memory by an application, this would normally be called by the [[porting::startupcode|startup] code just before exiting.

	void freeall_far()


*The _far suffix is used to distinguish between near and far allocation methods.*

# "crt" functions

The compiler will generate various calls to functions whenever data is required to be accessed from a far location, these are used to pick up the various primitive datatypes from far memory (it has no knowledge of the actual implementation of far memory).

### Reading routines

These have entry ehl = memory pool address:

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

### Writing routines

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

# String Routines

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

