## Preprocessor
[[Top](Tool---z80asm)]

**z80asm** runs a preprocessor on the input text and assembles the result 
output. The preprocessor understands the following statements:

### #define

`#define` works like in the C preprocessor and allows the definition of
assembly macros. The macro definition may include `\` which is translated 
to a newline. The macro definition can be continued on a subsequent line
if the previous line ends with `\`.

The macro name and the formal parameters names follow the rules of a C
identifier. They can start with a prefix `[.#$%@]`, usefull to 
allow token concatenation, e.g.:

    #define cat(#a, #b) #a#b
    cat(aa,bb): ret         ; -> aabb: ret


### #undef

`#undef` works like in the C preprocessor and allows a macro definition
to be deleted, if it exists.

