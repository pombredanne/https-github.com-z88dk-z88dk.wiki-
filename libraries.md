# Using CP/M libraries in REL format with z88dk

To import CP/M binary code in your application a bit of manuality is necessary.

First of all you need to extract the single object modules from the REL library:

    rel2z80 `<library.REL>`

We suggest to launch it in a separate folder because it could create many '.O' files.
Then you can put them all together in a single library with the command:

    z80asm -d -ns -nm -Mo -xlibrary FILE.O FILE2.O FILE3.O   ....

The current version of the converter might introduce errors, and there is not an easy way to know the parameter passing mechanism needed by the functions you are going to link.

