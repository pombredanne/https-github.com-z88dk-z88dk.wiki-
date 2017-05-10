# The COPT optimizer

The zcc command automatically involves the copt optimizer.   The level of optimization is defined by the "-O`<n>`" flag, in which `<n>` defines the number of passes and consequently the quantity of rules wich will be applied.

The optimization level is '2', but raising to '3' shouldn't have negative effects at all:

        zcc -O3 [flags]  [files to be compiled/linked]

Excluding the optimizer (-O0) is normally a bad idea.


## Optimizer rules

### Files

The ruleset files are located in:

        {z88dk}/lib/z80rules.2
        {z88dk}/lib/z80rules.1
        {z88dk}/lib/z80rules.0

### Syntax

Every rule is separated by a blank line.   Then a number of 'matching lines' is defined along with a 'transformed block, separated by the equal sign:

		ld	hl,%1	;const
		ld	(%2),hl
		ld	hl,0	;const
	=
		ld	hl,%1	;const
		ld	(%2),hl

The compiler generated code includes comments which help the rules avoiding to be too aggressive.


### Examples

		jr	z,ASMPC+3
		scf
		call	c,%2
	=
		call	nz,%2


