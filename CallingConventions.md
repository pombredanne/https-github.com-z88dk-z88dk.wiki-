# Calling conventions and Function decorators

z88dk provides several different conventions for calling functions. Understanding these will help linking your program with assembler functions and also aid debugging problems.

The conventions and modifiers are indicated to the compiler by adding them as a suffix to the function prototype, for example:

> int function(long val) __z88dk_callee;

z88dk supports the following calling conventions:

|  Decorator|Stack cleanup  | Description|
|--|--|--|
|__smallc  | Caller  | The default calling convention used by sccz80. The parameters are pushed onto the stack from left to right. For sccz80, a char is pushed onto the stack as word. A bug in zsdcc means that chars are pushed as a single byte when accessing `__smallc` functions. Return values are in hl, or dehl. |
|__stdc | Caller | The parameters are pushed in reverse order (from right to left). A char is pushed onto the stack as word. Return values are in hl or dehl.|
|__z88dk_sdccdecl | Caller | The default convention for zsdcc. The parameters are pushed onto the stack from right to left. A char is pushed onto the stack as a single byte. Return values are held in l, hl, or dehl.|

### Calling convention modifiers

|  Decorator|  Description|
|--|--|
|__z88dk_callee| The function being called (callee) is responsible for cleaning up the stack. |
|__z88dk_fastcall | At most a single parameter is passed in dehl. For __smallc it will be the rightmost parameter. For __stdc and __z88dk_sdccdecl it will be the only parameter.|
|_z88dk_saveframe|Valid for sccz80 generated code only. The sdcc framepointer (ix) will be saved on entry to the function. This is required if it is expected that this function will be called from sdcc compiled code and it uses either longs or floating point.
|__critical| The interrupt state is saved on entry to the function and restore on exit. |
|__naked|Used for assembler functions to ensure that the function prologue and epilogue is not generated. |

### Far convention modifiers

|  Decorator|  Usage|
|--|--|
| __z88dk_shortcall(RR, VV) |  Allows a function to be called via a rst RR trampoline. If value < 256 then generated code is rst RR; defb VV, otherwise rst RR; defw VV. This can be used to access functions located in non-paged memory banks. |
| __z88dk_params_offset(VV) |  When called via a trampoline it is likely that the parameters for a function will not be located starting at sp+2. This annotation defines the additional offset required to reach the parameters.   | 

With __z88dk_shortcall() only direct calls will use the rst trampoline, calling via a function pointer will call as usual. Thus, when shipping a library or providing an interface you will need to provide a matching function stub which will call via the rst trampoline.

### Miscellaneous modifiers

|  Decorator|  Usage|
|--|--|
| __preserves_regs(r1,r2...) |Indicates to sdcc that the specified registers are preserved by the function. This information is used by sdcc to improve the quality of the code generated.| 

### sccz80 library annotation

By convention, library functions that are used by sccz80 are marked with `__LIB__` following
the type, for example:

    void __LIB__ mylibrary_function(int param);

This means that sccz80 will call `mylibrary_function` and sdcc will call `_mylibrary_function`,
these different entry points can be useful for adjusting calling convention (essential for
vaargs functions).


