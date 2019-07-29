## Z80 module assembler / linker / librarian (z80asm)

**z80asm** is part of the [z88dk](http://www.z88dk.org/) project and is used as the back-end of the z88dk C compilers. It is not to be confused with other non-**z88dk** related projects with the same name.

**z80asm** is a relocatable assembler, linker and librarian that can assemble Intel 8080/8085 and Z80-family assembly files into a relocatable object format, can manage sets of object files in libraries and can build binary images by linking these object files together. The binary images can be defined in different sections, to match the target architecture.

This manual contains the description of features that are under development and not yet available. These are marked [TO-DO].

## Usage ...

### ... as assembler

```
z80asm [options] file...
```

By default, i.e. without any options, **z80asm** assembles each of the listed files into relocatable object files with a ```.o``` extension. It shows a summary of all the options when called with the ```-h``` option.

### ... as linker

```
z80asm -b [options] file...
```

When called with the ```-b``` option, **z80asm** links the object files together into a set of binary files.

### ... as librarian

```
z80asm -xlibrary.lib [options] file...
```

When called with the ```-x``` option, **z80asm** builds a library containing all the object files passed as argument. That library can then be used during linking by specifying it with the ```-i``` option.

## Contents

* [z80asm Input Format](Tool-z80asm-input-format)
* [z80asm Object File Format](Tool-z80asm-object-file-format)

## Copyright

The original z80asm module assembler was written by Gunther Strube. 
It was converted from QL SuperBASIC version 0.956, initially ported to Lattice C,
and then to C68 on QDOS.

It has been maintained since 2011 by Paulo Custodio.

Copyright (C) Gunther Strube, InterLogic 1993-99  
Copyright (C) Paulo Custodio, 2011-2019

## License

Artistic License 2.0 [http://www.perlfoundation.org/artisticlicense2_0](http://www.perlfoundation.org/artisticlicense2_0 "Artistic License 2.0")
