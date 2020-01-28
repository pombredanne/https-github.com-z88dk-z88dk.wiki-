======= Importing object code from the CP/M REL format =======

**Note: this is a topic for advanced users.  The tools are still at an experimental stage, and the conversion might not be successful**


The REL format is an ultra-compact object code format widely used in the CP/M environment introduced by Microsoft, most probably inspired by the DEC world, where a REL format existed.

In the eighties, the REL format was used by many compiled languages and assemblers, notably the m80 and l80 assembly tools.
The scope of the Z88DK REL tools is to permit the developers to import cross-developed modules or even to cross-link alien code to a new self developed runtime library for new z80 based platforms.

In the future a direct support of this format might be added to z80asm.


# The rel2z80 library conversion tool

This is the main support tool.
It extracts from a single .REL file every object module creating separate Z80ASM compliant files.


# The rel2bin debugging tool

This tool extract the object modules doing a sort of pseudo-linking starting from address at F000.
Its text output helps to understand cross-references between the modules.
You might want to disassemble them, I.E. with dz80: in that case a batch command could help.
See the following example, for the MS-DOS command prompt:

```
rel2bin %1 > %1.txt
md %1.dir
move /y *. %1.dir
move /y %1.txt %1.dir
cd %1.dir
copy *. *.bin
for %%x in (*.) do ..\dz80 %%x
del *.
del *.bin
```


