 ====== Translation of C Names to ASM Names (Accessing C Variables from z80 Asm) ======



### Example

	
	
	#include "stdio.h"
	
	extern char mydata[];
	
	#asm
	._mydata
	 defm	"This is a string"
	 defb	0 ; String termination
	#endasm
	
	main()
	{
	  printf("%s",mydata);
	
	}
	
	
	



