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
    long <<= shift_count

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

## When possible use global variables (sccz80)

Access to local variables is slightly slower and the generated code becomes slightly bigger. However...

## Declare most frequently used variables last (sccz80)

sccz80 can optimise access to variables near the top of the stack

## Avoid inserting debugging code (sccz80)

The options `--c-code-in-asm` and `-Cc--gcline` insert extra content into the output assembler. Although this can aid understanding the output, it will affect intra-statement redundant code elimination that is performed by the peephole stage.

## Use the `__z88dk_fastcall` calling convention (sccz80, zsdcc)

Functions that take a single parameter can be declared as fastcall. This can save the overhead of pushing the parameter
for each function call. See [calling conventions](CallingConventions) for more details.

## Use `__z88dk_callee` calling convention (sccz80, sdcc)

Most of the z88dk library is usese the 'FASTCALL' or 'CALLEE' modes to save memory and execution time. See [calling conventions](CallingConventions) for more details.

## Do not reinvent the wheel

The z88dk function libraries are mostly written in assembly code; this helps saving a lot of memory and execution time; avoid using an equivalent C implementation of the existing functions, if any.

## Split your libraries in modules

The linker is able to link in code portions incrementally, adding them only when they are really used, no matter if they were invoked already by the "header file" declarations or by the assembly code equivalents.

# Classic Library

## Disable std*

Disabling stdio can be useful when memory is tight. To disable it add the option `-pragma-define:CRT_ENABLE_STDIO=0`. Even when stdio is disabled you can still interact with the console with a few code substitutions:

```
printf() -> printk()
getchar() -> fgetc_cons()
putchar() -> fputc_cons()
```

## Switch to an alternate console driver

Depending on the target, the console driver may be consuming a large proportion of program space. In particular, the `ansiterminal` is quite large. In general the option `-pragma-redirect:fputc_cons=fputc_cons_native` will select the native console driver which is usually the most compact. However, the native driver is usually dependent on the targets ROM and may not offer sufficient formatting controls for your program, as a compromise, if the generic console is available for your machine `-pragma-redirect:fputc_cons=fputc_cons_generic` will offer portable formatting controls and typically consume around 3-400 bytes.

## Don't initialise BSS memory

By default, the classic library will initialise BSS memory to 0 on startup. You can save 13 bytes by using option `-pragma-define:CRT_INITIALIZE_BSS=0`


## Disable/reduce the atexit stack

If your program never exits or you don't register atexit() functions then you can adjust the size of the atexit() stack using: `-pragma-define:CLIB_EXIT_STACK_SIZE=0` or any size that you choose.