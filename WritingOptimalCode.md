# sdcc

SDCC is an optimising compiler and will perform cse and other standard optimisation techniques, the degree of optimisation performed is controlled by the `--max-allocs-per-node` option. The recommended value for good results is 200000, thus give the option `--max-allocs-per-node200000` to the zcc command line.

However, this degree of optimisation takes a considerable amount of time - it's not unheard of zsdcc to spend 30 minutes optimising a file, consuming a whole core in the process.

# sccz80

## Making code faster without rewriting it

The sccz80 compiler does not perform any optimisation beyond (limited) constant folding and dead path elimination. When generating code it attempts to achieve a practical balance between code speed and code size. Optionally you can enable all longer replacements on the command line using `--opt-code-speed`. For example, to enable longer replacements for add32, sub32 then provide the compile option `--opt-code-speed=sub32,add32`

| Parameter | Default | Effect | +bytes | -T |
|-|-|-|-|-|
| all | No | Enable all replacements that make code quicker, but larger |-| - |
| add32 | No | Inline 32 bit addition| +4 (+3) | -39T (-44T/-6T) |
| sub32 | No | Inline 32 bit subtraction | +10 (+3) | -39T (-26T/-10T) |
| sub16 | No | Inline 16 bit subtraction (costs 1 byte) | +1 | -27T |
| lshift32 | Yes | Inline longer 32 bit left shifts | - | - |
| rshift32 | Yes | Inline longer 32 bit right shifts | - | - |
| intcompare | No | Inline some 16 bit comparisons | - | - |
| longcompare | No | Inline 32 bit equality | - | - |
| inlineints | No | Inline getters and setters for 16 bit variables | get: +1, set +2 | -27T |
| ucharmult | No | Inline uint8 * uint8 multiplication | +8 | 155T (average) |

### Sample results

Sample program calculates an md5sum of a file, it uses long operations heavily. The following show the effect of various options on the result (Initial is from sccz80 on 5/9/2017). These numbers were obtained around about 04/2018 - there have been some changes since then.

| Compile Flags | T states | File size |
|-|-|-|
| Initial:                               | 66043689      |  13668 |
| -O0                                    | 54900142      |  14601 |
| -O1                                    | 47600352      |  13586 |
| -O2 --opt-code-speed=none              | 48017745      |  13429 |
| -O2                                    | 47449367      |  13537 |
| -O2 --opt-code-speed=inlineints        | 45873898      |  13460 |
| -O2 --opt-code-speed=all               | 43662158      |  14311 |
| -O2 --opt-code-speed=all -Cc-unsigned  | 43661908      |  14270 |
| -O3                                    | 48324873      |  13423 |
| -compiler=sdcc                         | 38017961      |  19581 |
| -compiler=sdcc -SO3                    | 37560042      |  19233 |

It's clear that there is a balance between size and speed to be made, for this particular application, `-O2 --opt-code-speed=inlineints` is probably the best compromise option when using sccz80

## Making code smaller

If memory is really tight, then compiling with  -O3 will replace common code sequences with calls to functions that achieve the same thing. This can reduce the code size, but at the cost of decreased execution speed (as a result of the inserted call/ret).

# Tips

## Use unsigned where possible (sccz80, zsdcc)

Dealing with unsigned values is much quicker on a z80 than dealing with signed values and more efficient code will be generated. In particular in the case of `for ( i = 0; i < 10; i++)`, if i is an unsigned int then the condition check is more efficient than if it were to be signed. Changing to an unsigned char will perform even better.

## Prefer `unsigned char` (sccz80, zsdcc)

zsdcc enables `unsigned char` by default and signed char has to be enabled with `-fsigned-char` or explicitly typed with the code. Both compilers generate better code if char is unsigned.

## Switch on a char (sccz80)

Switching on a char will generate smaller and faster code than switching on another datatype.

## Operate with a constant value (sccz80)

For the contrived example:

    int shift_count;
    long value;

    if ( condition ) {
       shift_count = 8;
    } else {
       shift_count = 16;
    }
    value <<= shift_count

will be faster if written:

    long value;

    if ( condition ) {
       value <<= 8;
    } else {
       value <<= 16;
    }

## Constant ordering (sccz80)		
		
    int a;		
		
    a = 3 - a + 2;		
		
Will generate worse code than:		
		
    int   a;		
		
    a = 3 + 2 -a;		


## Prefer pre- to post-increment/decrement (sccz80)

The post-increment operator requires the compiler to decrement the value, not all cases of this can be eliminated, so in general prefer the pre-increment version.

## When possible use static variables (sccz80/zsdcc)

Use static variables when reasonable. The z80 is not very good at stack-relative addressing. Neither of the two main methods (using an index register set to sp and offsetting from that, computing offsets from sp using hl) leads to particularly compact or efficient z80 code. A large improvement in code size and speed can be had from changing local variables to statics. Keep in mind that doing this means functions will no longer be reentrant. For sdcc, unsigned char variables and frequently accessed small variables can be an exception to this advice

## Avoid long lists of function parameters (sccz80/zsdcc)

Function parameters are located on the stack and, like the local variables mentioned in the last point, cannot be efficiently addressed by the z80. If long lists are unavoidable, chances are the function is also long. In these circumstances it can make sense to copy function parameters into local static variables before being used by the function.

## Use types of appropriate size (sccz80/zsdcc)

The z80 can do 8-bit and 16-bit arithmetic efficiently. 32-bit arithmetic involves many more cycles.

## Demote larger types to smaller types as soon as possible (sccz80/zsdcc)

If the program performs operations on large types and then stores results into smaller types, try to demote the larger types to the smaller type and carry out the operations on that.

## Declare most frequently used variables last (sccz80)

sccz80 can optimise access to variables near the top of the stack

## Avoid inserting debugging code (sccz80)

The options `--c-code-in-asm` and `-Cc--gcline` insert extra content into the output assembler. Although this can aid understanding the output, it will affect intra-statement redundant code elimination that is performed by the peephole stage.

## Use the `__z88dk_fastcall` calling convention (sccz80, zsdcc)

Functions that take a single parameter can be declared as fastcall. This can save the overhead of pushing the parameter
for each function call. See [calling conventions](CallingConventions) for more details.

## Use `__z88dk_callee` calling convention (sccz80, sdcc)

Most of the z88dk library is usese the `__z88dk_fastcall` or `__`z88dk_callee` modes to save memory and execution time. See [calling conventions](CallingConventions) for more details.

## Compute things once and store the result (sccz80/zsdcc)

It's not uncommon for modern code to repeatedly recompute expressions that evaluate to the same value. A common place where this is done is in the conditional of loops. Removing redundant calculations will not only speed up code, but it will give the compilers a better chance at generating better code.

## Assign data to appropriate sections (sccz80/zsdcc)

This applies to rom targets. Non-zero initial data must have a copy stored in rom so that the crt can initialize ram at startup. Consider two different declarations of a large array holding the text of a book. One is done with 'char book[] = ”…”;' and the other with 'char *book = ”…”;'. The array implies that the book data is modifiable so it is assigned to the DATA section and two copies will be present at runtime – the stored copy in rom and the active copy in ram. The second declaration stores the book text in a string constant. String constants are read-only so it will be assigned to CODE/RODATA and at runtime only one copy of the string will exist in rom, freeing up ram in comparison to the other declaration. Judicious use of the const qualifier can also affect whether data is stored in the DATA section or the CODE/RODATA section. Keep in mind that the stored DATA section can be compressed so if there is more ram than rom available, it may be preferable to store in the DATA section even though two copies would be present at runtime (the rom would contain a much smaller compressed copy)

## Do not reinvent the wheel

The z88dk function libraries are mostly written in assembly code; this helps saving a lot of memory and execution time; avoid using an equivalent C implementation of the existing functions, if any. If you think your similar c or asm code is better suited to the task and will perform faster or will be smaller, test it.

## Split your libraries in modules

The linker is able to link in code portions incrementally, adding them only when they are really used, no matter if they were invoked already by the "header file" declarations or by the assembly code equivalents.

# Classic Library

## Disable std*

Disabling stdio can be useful when memory is tight. To disable it add the option `-pragma-define:CRT_ENABLE_STDIO=0`. Even when stdio is disabled you can still interact with the console with a few code substitutions:

```
printf() -> printk()
getchar() -> fgetc_cons()
putchar() -> fputc_cons()
puts() -> puts_cons()
```

## Switch to an alternate console driver

Depending on the target, the console driver may be consuming a large proportion of program space. In particular, the `ansiterminal` is quite large. In general the option `-pragma-redirect:fputc_cons=fputc_cons_native` will select the native console driver which is usually the most compact. However, the native driver is usually dependent on the targets ROM and may not offer sufficient formatting controls for your program, as a compromise, if the generic console is available for your machine `-pragma-redirect:fputc_cons=fputc_cons_generic` will offer portable formatting controls and typically consume around 3-400 bytes.

## Disable unused graphics modes

Several ports support multiple graphics mode, disabling the modes you don't use can help save memory. More details can be found on the port page if supported, or raise an issue and we'll add some options to disable them.

## Link to the dummy fcntl library

Some ports implicitly link to functioning fcntl functions. If you don't use them then they can be replaced with non-functional stubs by adding `-lndos` to the compile line.

## Switch maths library implementations

The different maths libraries have differing memory and performance profiles depending on what you're doing. Switching to libraries that utilise firmware floating point can save up to 2-3k of memory.

## Don't initialise BSS memory

By default, the classic library will initialise BSS memory to 0 on startup. You can save 13 bytes by using option `-pragma-define:CRT_INITIALIZE_BSS=0`


## Disable/reduce the atexit stack

If your program never exits or you don't register atexit() functions then you can adjust the size of the atexit() stack using: `-pragma-define:CLIB_EXIT_STACK_SIZE=0` or any size that you choose.

# Newlib

## Ensure that the minimal crt required is selected

Targets normally supply more than one crt option that can be selected by number on the compile line with ”-startup=n”. These crts vary in options that can consume different amounts of memory. In particular, if your program does not use stdin, stdout or stderr, choose a crt that does not instantiate any devices at startup.
Opt out of stdio if it's not needed. Use of printf and scanf implies that terminal i/o drivers are required that implement line editing, windows, terminal emulation and so on. This is a lot of extra code that is not always required for all projects. Most embedded applications provide their own i/o subroutines and communicate directly with devices. In these cases, a full-blown stdio implementation is wasted. By selecting a crt that does not instantiate terminal devices, programs will not have that extra code included. Keep in mind that they can still use functions from the sprintf and sscanf families to operate on buffers read from or written to devices. They can also use memstreams to do file i/o to memory buffers.

## Modify the crt to change font

If the program doesn't use the default font supplied by the crt, change it so that the default font is not stored as part of the binary.

## Configure the library and crt

Configure the library to choose a speed and space compromise suitable to your project. In particular, opt out of individual printf and scanf converters that your program does not use. Disable unused options in the crt that occupy memory space in the resulting binary. In particular, eliminate/resize the malloc heap and stdio heap if they are not needed.