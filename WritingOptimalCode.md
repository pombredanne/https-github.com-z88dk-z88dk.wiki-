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
| lshift32 | Yes | Inline 32 bit left shits | - | - |
| rshift32 | Yes | Inline 32 bit right shits | - | - |
| intcompare | No | Inline some 16 bit comparisons | - | - |
| longcompare | No | Inline 32 bit equality | - | - |
| inlineints | No | Inline getters and setters for 16 bit variables | get: +1, set +2 | -27T |

## Making code smaller

If memory is really tight, then compiling with  -O3 will replace common code sequences with calls to functions that achieve the same thing. This can reduce the code size, but at the cost of decreased execution speed (as a result of the inserted call/ret).

# Tips

## Use unsigned where possible (sccz80, zsdcc)

Dealing with unsigned values is much quicker on a z80 than dealing with signed values and more efficient code will be generated. In particular in the case of `for ( i = 0; i < 10; i++)`, if i is an unsigned int then the condition check is more efficient than if it were to be signed. Changing to an unsigned char will perform even better.

## Prefer `unsigned char` (sccz80, zsdcc)

zsdcc enables `unsigned char` by default and signed char has to be enabled with `-fsigned-char` or explicitly typed with the code. Both compilers generate better code if char is unsigned.

## Switch on a char (sccz80)

Switching on a char will generate smaller and faster code than switching on another datatype.

## Constant ordering (sccz80)

    int a;

    a = 3 + a + 2;

Will generate worse code than:

    int   a;

    a = a + 3 + 2;

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

## Prefer pre- to post-increment/decrement (sccz80)

The post-increment operator requires the compiler to decrement the value, not all cases of this can be eliminated, so in general prefer the pre-increment version.

## When possible use global variables (sccz80, zsdcc)

Access to local variables is slightly slower and the generated code becomes slightly bigger.

## Avoid inserting debugging code (sccz80)

The options `--c-code-in-asm` and `-Cc--gcline` insert extra content into the output assembler. Although this can aid understanding the output, it will affect intra-statement redundant code elimination that is performed by the peephole stage.





