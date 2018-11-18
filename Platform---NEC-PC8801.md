
# Quick start

    zcc +pc88 -create-app -Cz--audio program.c


This will generate three files, **a.bin** (a pure binary block), **a.t88** (a file valid on many emulators), and **a.wav**, never tested but meant to be loaded on the real hardware.



# Emulator notes

An emulator capable of loading the T88 cassette file format is necessary, the default ORG address is $8A00, it was chosen peeping into the original software written in the eighties.

To run an appmake created program, first start N88 BASIC (on the M88 emulator the cpu clock must be set to 8mhz) or N80 BASIC, get to the prompt (answering first to the question about the "files?" number), then:

    MON
    R   (or L if you are using the N80 BASIC)
    G8A00

(CTRL-b would return to BASIC from monitor)


# Appmake extras

The audio file can be slightly accelerated with the -Cz--fast build option.

Appmake can be also run in 'dumb mode' to convert any T88 file to wav:

    appmake -b program.t88 --audio --fast --dumb

## M88 emulator

Please note that the M88 emulator and possibly also others require the correct CPU speed setting (usually 4 or 8 Mhz) to load correctly the program from the tape file.

