# Calling conventions and Function decorators

z88dk provides several different conventions for calling functions. Understanding these will help linking your program with assembler functions and also aid debugging problems.

The conventions and modifiers are indicated to the compiler by adding them as a suffix to the function prototype, for example:

    int function(long val) __z88dk_callee;

z88dk supports the following calling conventions:

|  Decorator|Stack cleanup  | Description|
|--|--|--|
|`__smallc`  | Caller  | The default calling convention used by sccz80. The parameters are pushed onto the stack from left to right. For sccz80, a char is pushed onto the stack as word. **Note** A bug in zsdcc means that chars are pushed as a single byte when accessing `__smallc` functions. Return values are in hl, or dehl. |
|`__stdc` | Caller | The parameters are pushed in reverse order (from right to left). A char is pushed onto the stack as word. Return values are in hl or dehl.|
|`__z88dk_sdccdecl` | Caller | The default convention for zsdcc. The parameters are pushed onto the stack from right to left. A char is pushed onto the stack as a single byte. Return values are held in l, hl, or dehl.|

### Calling convention modifiers

|  Decorator|  Description|
|--|--|
|`__z88dk_callee` | The function being called (callee) is responsible for cleaning up the stack. |
|`__z88dk_fastcall` | At most a single parameter is passed in registers. For __smallc it will be the rightmost parameter. For `__stdc` and `__z88dk_sdccdecl` it will be the only parameter. The register used is always a subset of DEHL depending on the parameter bit width.  So, for example, an integer would be passed in the HL register and a long in DEHL.  sccz80's floats / doubles are 48-bit and are treated a little differently.  They are passed via the "primary floating point accumulator".  In the classic C library this is six bytes of static memory labelled "fa".  In the new C library this is the registers BCDEHL' in the exx set.  This means the classic C library's floating point implementation is not re-entrant whereas the new C library's is.  sdcc's 64-bit long long type cannot be passed using fastcall linkage.|
|`__z88dk_saveframe`|Valid for sccz80 generated code only. The sdcc framepointer (ix) will be saved on entry to the function. This is required if it is expected that this function will be called from sdcc compiled code and it uses either longs or floating point.
|`__critical`| The interrupt state is saved on entry to the function and restore on exit. |
|`__naked`|Used for assembler functions to ensure that the function prologue and epilogue is not generated. |

### Banked calling convention modifiers

|  Decorator|  Usage|
|--|--|
| `__banked` | Allows a function to be called via the target specific `banked_call` function. The call is followed by a 32 bit address of the function. The first two bytes are the address, and the second two are the bank. Function examples of this calling convention can be found in the classic +zx, +msx and +gb ports. |
| `__z88dk_shortcall(RR, VV)` |  Allows a function to be called via a rst RR trampoline. If VV < 256 then generated code is rst RR; defb VV, otherwise rst RR; defw VV. This can be used to access functions located in non-paged memory banks. |
| `__z88dk_params_offset(VV)` |  When called via a trampoline it is likely that the parameters for a function will not be located starting at sp+2. This annotation defines the additional offset required to reach the parameters.   | 

With `__z88dk_shortcall()` only direct calls will use the rst trampoline, calling via a function pointer will call as usual. Thus, when shipping a library or providing an interface you will need to provide a matching function stub which will call via the rst trampoline.

### Miscellaneous modifiers

|  Decorator|  Usage|
|--|--|
| `__preserves_regs(r1,r2...)` |Indicates to sdcc that the specified registers are preserved by the function. This information is used by sdcc to improve the quality of the code generated.| 

### sccz80 library annotation

By convention, library functions that are used by sccz80 are marked with `__LIB__` following
the type, for example:

    void __LIB__ mylibrary_function(int param);

This means that sccz80 will call `mylibrary_function` and sdcc will call `_mylibrary_function`,
these different entry points can be useful for adjusting calling convention (essential for
vaargs functions).

## Return values

Return values are held in a subset of DEHL depending on the return value's bit width.  HL would hold an integer return value whereas DEHL would hold a long return value.  sccz80's 48-bit float / double is treated differently with the classic library returning its value in the "primary floating point accumulator" which is six bytes of static memory at address "fa" and the new c library returning by registers BCDEHL' in the exx set.  Note the same rules apply as for fastcall linkage where a single parameter is passed to a function via DEHL.

Return of the 64-bit long long type is handled specially.  The compiler will pass a pointer to memory for the return value as the first parameter in the function call.  This parameter is not listed in the function prototype.  The called function must use that pointer to store the 64-bit value returned.


