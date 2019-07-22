# Z80 module assembler / linker / librarian (z80asm)

**z80asm** is part of the [z88dk](http://www.z88dk.org/) project and is used as the back-end of the z88dk C compilers. It is not to be confused with other non-**z88dk** related projects with the same name.

**z80asm** is a relocatable assembler, linker and librarian that can assemble Intel 8080/8085 and Z80-family assembly files into a relocatable object format, can manage sets of object files in libraries and can build binary images by linking these object files together. The binary images can be defined in different sections, to match the target architecture.

# Usage

```
z80asm [options] file...
```

By default, i.e. without any options, assembles each of the listed files into relocatable object file with a ```.o``` extension. It shows a summary of all the options when called with the ```-h``` option.

# Input line format

Assembly code is written in text lines with an optional label field, an optional opcode field and a list of arguments for the opcode. A comment starts with a semi-column (```;```) and ends at the end of the line.

Differently to most other assemblers, white space is not significant, i.e. a label can be defined after white space, and an opcode can be written at column 1.

Symbols must start with a letter or underscore character (```_```), and can have letters, digits or underscores. Labels are case-sensitive.

A label is defined by prefixing a symbol with a dot (```.```) or suffixing it with a colon (```:```). The following examples are label definitions:

```.A_Label```  
```AnotherLabel:```  

# Output files

The object and library files produced by *z80asm* are documented in [z80asm Object File Format](Tool-z80asm-object-file-format).

## Copyright

The original z80asm module assembler was written by Gunther Strube. 
It was converted from QL SuperBASIC version 0.956, initially ported to Lattice C,
and then to C68 on QDOS.

It has been maintained since 2011 by Paulo Custodio.

Copyright (C) Gunther Strube, InterLogic 1993-99  
Copyright (C) Paulo Custodio, 2011-2017

## License

Artistic License 2.0 [http://www.perlfoundation.org/artisticlicense2_0](http://www.perlfoundation.org/artisticlicense2_0 "Artistic License 2.0")
