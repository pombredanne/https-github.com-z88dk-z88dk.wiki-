All the compiled procedural languages need a way to pass parameters to the functions.

The z88dk parameter passing mechanism normally relies on the stack: local variables declared in a C program are allocated on the stack as a function is entered and are popped off the stack as functions are exited. z88dk supports [multiple calling conventions](CallingConventions) so care should be taken when writing assembler that interfaces with C.

Such parameters are dynamic (in that memory is allocated and deallocated from the stack as required) and transient (in that referring to their addresses has no meaning after the function terminates).

sccz80 makes no assumptions about registers that need to be preserved, however the target platform may. For example on the ZX Spectrum, the iy register should not be disturbed unless you are using your own interrupt manager and you don't call any ROM routines.

**Example**:  accessing local variables from assembler embedded in a C function.

    int myfunc(char b, unsigned char *p)
    {
      #asm
      
      ld hl,2
      add hl,sp              ; skip over return address on stack
      ld e,(hl)
      inc hl
      ld d,(hl)              ; de = p
      inc hl
      ld a,(hl)              ; a = b, "char b" occupies 16 bits on stack
                             ;  but only the LSB is relevant
      ...
      
      ld hl,1                ; hl is the return parameter
      
      #endasm
    }

passing two 8-bit values share a 16-bit stack location.

    void myfunc(char x, char y)
    {
      #asm
      
      ld hl,2
      add hl,sp              ; skip over return address on stack
      ld e,(hl)              ; e = y
      inc hl
      ld c,(hl)              ; c = x
      ...
            
      #endasm
    }

At the call location the sp is manipulated as follows:

    ld l, 'x'
    ld a, 'y'
    push hl
    inc  sp
    push af
    inc  sp


A "long" parameter takes 4 bytes on the stack; the following example could be used in case of a single long parameter being passed to a function, loading it in the **DE-HL** word pair :

    my_function:
        pop     bc
        pop     hl
        pop     de
        push    bc      ;return address to program
        ...
        ...

### Exceptions

#### `__z88dk_fastcall` qualifier

z88dk's special `__z88dk_fastcall` qualifier can be used if the C function has just one parameter.
In that case, the parameter is passed in the HL register pair instead of being allocated on the stack.

    int myfunc(unsigned char *p) __z88dk_fastcall __naked
    {
      #asm
      
      ; hl = p
      
      ...
      
      ; return value in hl
      
      #endasm
    }


#### Floating Point numbers




### Calling C functions from within an assembly program

An assembly written function needs to get the parameters from the stack; in example, here is how the "plot" function recovers the X and Y coordinates (using only the lower byte of the int parameter):

    plot:
		ld	ix,0
		add	ix,sp
		ld	l,(ix+2)    ; Y coordinate
		ld	h,(ix+4)    ; X coordinate
		...
		...




### Including other language's functions

At a machine level, different languages can be mixed as long as they use the same data format and the same parameter passing mechanism.

The BDS C compiler, in example, uses a static frame, pointing to maximum 7 different parameters: in this case we need to provide a parameter conversion routine to make it work, which will pick the data from the stack and will put them in reverse order in pre-set positions:

	GLOBAL	arghak

	GLOBAL	arg1
	GLOBAL	arg2
	GLOBAL	arg3
	GLOBAL	arg4
	GLOBAL	arg5
	GLOBAL	arg6
	GLOBAL	arg7


    ;
    ; This routine takes the first 7 args on the stack
    ; and places them contiguously at the "args" ram area.
    ; This allows a library routine to make one call to arghak
    ; and henceforth have all it's args available directly
    ; through ld hl,() instead of having to hack the stack as it
    ; grows and shrinks. Note that arghak should be called as the
    ; VERY FIRST THING a function does, before even pushing BC.
    ;

    arghak:	LD	DE,args	;destination for block move in DE
		LD	HL,4	;pass over two return address
		ADD	HL,sp	;source for block move in HL
		PUSH	BC	;save BC
		LD 	b,14	;countdown in B
    arghk2:	LD 	a,(HL)	;copy loop
		LD	(DE),A
		INC	HL
		INC	DE
		DEC	b
		JP	NZ,arghk2	
		pop	BC	;restore BC
		ret

    args:
    arg1:
		defw	0
    arg2:
		defw	0
    arg3:
		defw	0
    arg4:
		defw	0
    arg5:
		defw	0
    arg6:
		defw	0
    arg7:
		defw	0




