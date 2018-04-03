Ticks is a command line CPU emulator that can be used for testing and debugging algorithms and code. The classic library can compile binaries that run on ticks using the +test target.

## Usage

In it's simplest case, launch ticks with:

    ticks [binary file]

It will run for a around 10,000,000 ticks which may not be sufficient, to increase the time it will run use the `-w` option:

    ticks -w X [binary file]

Where X counts the number of 400,000,000 cycles to wait before exiting.

To specify command line options (which are picked up main in the `argc` and `argv` parameters invoke ticks as follows:

    ticks [binary file] -- [argv0] [argv1]

## CPU Features

Ticks supports emulating the z80 (default), z180 (-mz180), ZX Next z80 extensions (-mz80-zxn) and Rabbit processors (-mr2k), reporting accurate timing information for each target. _Note:_ Not all Rabbit instructions may be emulated.

## Debugging

Ticks provides a command line debugger, this can be launched as follows:

    ticks -d -x [map file] [binary file]

Specify `-x [map file]` is optional, however specifying it allows symbolic debugging. Ticks will then sit at address 0 waiting for an input from you. Type `help` to view the available commands.

### Hotspot detection

Ticks can also report hotspots for code execution, launch the debugger, type `hotspots on` and then `continue`, on exiting a file called `hotspots` will be written in the current directory. This file reports the number of times an address has been executed along with a disassembly of that line. To order this in terms of frequency you can use the standard sort tool. For example:

    sort -nr hotspots

Will show the commonest hit addresses first of all.

## Stdio and File I/O

Ticks provides a full stdio that will output to the console, along side this file I/O is supported as well.
