# UNISTD.H

 | Include    | #include `<unistd.h>`                                                                                              |                                                  
 | -------------------------------------------------------------------------------------------------------------------------------                                                  
 | Header     | [{z88dk}/include/_DEVELOPMENT/sccz80/unistd.h](http://z88dk.cvs.sourceforge.net/viewvc/z88dk/z88dk/include/_DEVELOPMENT/sccz80/unistd.h?content-type=text%2Fplain) |
 | | [{z88dk}/include/_DEVELOPMENT/sdcc/unistd.h](http://z88dk.cvs.sourceforge.net/viewvc/z88dk/z88dk/include/_DEVELOPMENT/sdcc/unistd.h?content-type=text%2Fplain) |               

Other References:


*  [ GNU libc Manual](http://www.gnu.org/software/libc/manual/html_node/Low_002dLevel-I_002fO.html#Low_002dLevel-I_002fO )

*  [ The Open Group](http://pubs.opengroup.org/onlinepubs/009695399/basedefs/unistd.h.html )

## MACROS

 | NULL            | %%((void*)(0))%%  | pointer to nothing  |                         
 | -----------------------------------------------------------                         
 |                                                                                      |  |  | 
 | SEEK_SET        | 0                 | offset is an absolute file position  |        
 | SEEK_CUR        | 1                 | offset is relative to current file position  |
 | SEEK_END        | 2                 | offset is relative to EOF  |                  
 |                                                                                      |  |  | 
 | STDIN_FILENO    | 0                 | fd stdin is attached to  |                    
 | STDOUT_FILENO   | 1                 | fd stdout is attached to  |                   
 | STDERR_FILENO   | 2                 | fd stderr is attached to  |                   


## TYPES

 | intptr_t        | int           | signed integer that can hold a pointer value  |
 | ---------------------------------------------------------------------------------
 | off_t           | long          | file offset  |                                 
 | size_t          | unsigned int  | used for size of objects  |                    
 | ssize_t         | unsigned int  | used for count of bytes  |                     


## FUNCTIONS

### int close(int fd)

Clear the file descriptor *fd* in the file table making the file descriptor available for a newly opened file.  Decrease the file's reference count and if it reaches zero, flush and close the file.  Return 0 on success and -1 on failure.

If an error occurs, carry flag is set and errno is set to:


*  [EBDFD] if *fd* is not an open file

### int dup(int fd)

Duplicate the file descriptor *fd* into the first available slot in the file table in numerical order.  Returns the duplicate fd on success and -1 on failure.

If an error occurs, carry flag is set and errno is set to:


*  [EBDFD] if *fd* is not an open file

*  [ENFILE] if the file table is full

### int dup2(int fd, int fd2)

Duplicate the file descriptor *fd* into *fd2*.  If *fd2* is associated with an open file, close it first.  Returns *fd2* on success or -1 on failure.

If an error occurs, carry flag is set and errno is set to:


*  [EBDFD] if *fd* is not an open file

*  [EBDFD] if *fd2* is out of range

### void _exit(int status)

Same as _Exit(status)

### off_t lseek(int fd, off_t offset, int whence)

Adjust the file pointer by amount *offset* according to *whence*:

 | whence    | |
 | -------------
 | SEEK_SET      | Set the file pointer to *offset*                              | 
 | SEEK_CUR      | Adjust the current file pointer by signed *offset*            | 
 | SEEK_END      | Set the file pointer to the EOF position plus signed *offset* | 

Return the new file pointer position on success or -1 on error.

If an error occurs, carry flag is set and errno is set to:


*  [EBDFD] if *fd* is not an open file

*  [EINVAL] if *whence* is invalid

The driver may return other errors.

### ssize_t read(int fd, void *buf, size_t nbyte)

Read a maximum of *nbyte* bytes from the stream and write them into buffer *buf*.  Returns the number of bytes actually read or -1 on error.

If an error occurs, carry flag is set and errno is set to:


*  [EACCES] if *fd* is not open for reading

*  [EBDFD] if *fd* is not an open file

The driver may return other errors.

### ssize_t write(int fd, const void *buf, size_t nbyte)

Attempt to write *nbyte* bytes from buffer *buf* to the stream.  Return the number of bytes actually written or -1 on error.

If an error occurs, carry flag is set and errno is set to:


*  [EACCES] if *fd* is not open for writing

*  [EBDFD] if *fd* is not an open file

The driver may return other errors.

