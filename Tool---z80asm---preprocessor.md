## Preprocessor
[[Top](Tool---z80asm)]

**z80asm** runs a preprocessor on the input text and assembles the result 
output. The preprocessor understands the following statements:

### DEFL

Define a text macro that can be redefined and that can refer to it's previous 
value. A pure text-substitution is done, i.e. the text of the previous value 
is pasted in place in the new value.

	DEFL var = var+1	; var is now "+1"
	DEFL var = var+1	; var is now "+1+1"

Alternative syntax:

	var DEFL var+1

### EXITM

Exits the current macro expansion. It is normally used inside a conditional.

	MACRO m1 arg
	if arg==1
	  EXITM
	endif
	...
	ENDM

### LOCAL

Used inside macros to declare the following symbols as local to the macro
invocation, i.e. they are replaced by a unique identifier on each invocation.

	MACRO abc
	LOCAL l1,l2
	l1:					; replaced by l1__<n> unique identifier
	l2:					; replaced by l2__<n> unique identifier
	ENDM

### MACRO

Define a new macro.

	MACRO name [arg1,...]
	text
	ENDM

Creates a macro that is expanded when referred to in the opcode field of an 
instruction. The formal parameters are assigned to the actual arguments before
the expansion.

Alternative syntax:

	name MACRO [arg1,...]
	text
	ENDM

### #define

`#define` works like in the C preprocessor and allows the definition of
assembly macros. The macro definition may include `\` which is translated 
to a newline. The macro definition can be continued on a subsequent line
if the previous line ends with `\`.

The macro name and the formal parameters names follow the rules of a C
identifier. 

Example:

	#define pushall \
		push bc \ \
		push de \ \
		push hl
	pushall					; pushes bc, de and hl

Note the double role of the backslash, as a line continuation, and as a line 
break.

The token pasting operator ## can be used to glue together two tokens into one:

	#define cat(a, b) a ## b
	
	cat(aa,bb)				; expands to aabb

### #undef

`#undef` works like in the C preprocessor and allows a macro definition
to be deleted, if it exists.

