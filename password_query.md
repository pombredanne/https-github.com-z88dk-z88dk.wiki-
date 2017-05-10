# PQUERY.C

{{libnew:examples:pquery.png?317|pquery}}

This program demonstrates some of the features of the new C library's terminals.  It is a username / password query in a loop with the last entered username pushed into the input terminal's edit buffer to prime the user's input.  When the password is entered, the input terminal is placed in password mode which causes all echoed chars to be printed as asterisks.

The communication with the terminal device drivers is done with ioctl().  ioctl() is a general purpose device driver communicator that takes file descriptor, driver-defined message and parameters and forwards that message to the driver attached to the file descriptor.  At startup, as with unix-like systems, fd#0 is attached to stdin, fd#1 is attached to stdout and fd#2 is attached to stderr.

The input terminal's edit buffer is managed by a "[b_array_t](b_array_t)" which is a byte array type defined by the [adt library](adt library) and is very similar to C++'s vector`<char>` container but without the ability to grow.  It manages a fixed-size buffer (for the input terminal this is typically 64 bytes) and holds in its struct definition a pointer to the buffer, the size of the buffer (the number of chars stored) and the capacity of the buffer (max size of the buffer).  These details are copied out of the input terminal and into the local "b_array_t edit_buffer" variable using the "IOCTL_ITERM_GET_EDITBUF" ioctl.

Succeeding lines push the last username text into the input terminal's edit buffer by copying the username into its buffer and changing the local size member appropriately.  The following "IOCTL_ITERM_SET_EDITBUF" ioctl copies the local edit_buffer into the input terminal and an internal flag is set to indicate text was pushed.  This is a byte-wise shallow copy that involves the pointer to the edit buffer (this will be the same as the input buffer's pointer so no changes there), the size and the capacity of the buffer.  When manually changing the b_array_t's members like this, you do have to be careful to maintain the data type's invariants, for example don't write a string into the buffer that will be longer than the buffer can hold.  There is no danger of this happening in this program because the username is entered from the terminal so it can never be longer than the terminal's internal buffer.

The following scanf will cause the input terminal to first echo the pushed text and then resume line editing.  Effectively the input buffer is primed with the text pushed.

	:::c
	// zcc +zx -vn -clib=new pquery.c -o pquery
	// zcc +zx -vn -clib=sdcc_ix --max-allocs-per-node200000 --reserve-regs-iy pquery.c -o pquery
	// output binary has org 32768
	
	#include `<stdio.h>`
	#include `<stropts.h>`
	#include `<string.h>`
	#include `<adt/b_array.h>`
	
	b_array_t edit_buffer;
	
	char username[20];
	char password[20];
	
	main()
	{   
	   ioctl(1, IOCTL_OTERM_CLS);
	   
	   printf("USERNAME + PASSWORD QUERY\n");
	   printf("username pushed into edit buffer\n");
	   
	   // gather details about the input terminal's buffer
	   
	   ioctl(0, IOCTL_ITERM_GET_EDITBUF, &edit_buffer);
	   
	   while(1)
	   {
	      printf("\n=====\n\nUsername: ");
	
	      // push last username into edit buffer
	
	      fflush(stdin);
	      
	      edit_buffer.size = strlen(username);
	      strcpy(edit_buffer.data, username);
	      ioctl(0, IOCTL_ITERM_SET_EDITBUF, &edit_buffer);
	      
	      // read username
	      
	      scanf("%19[^\n]", username);
	      
	      // read password
	      
	      printf("Password: ");
	      
	      ioctl(0, IOCTL_ITERM_PASS, 1);
	      
	      fflush(stdin);
	      scanf("%19[^\n]", password);
	      
	      ioctl(0, IOCTL_ITERM_PASS, 0);
	      
	      // echo
	      
	      printf("\n\"%s\"\n", username);
	      printf("\"%s\"\n", password);
	   }
	}


The "fflush(stdin);" cause any pending chars in the input terminal to be dropped.  This is not guaranteed behaviour in all C implementations but it is defined behaviour in z88dk.  The first "fflush(stdin);" prior to setting the edit buffer is necessary to remove a potentially stored unget char. 

Normally it's better practice to use getline() to grab an entire line of text and then sscanf that as dealing with the stream directly is not something most C programmers are used to.  In particular scanf does not remove text from the stream that is rejected by a scanf converter and this has consequences that most programmers find unexpected.

