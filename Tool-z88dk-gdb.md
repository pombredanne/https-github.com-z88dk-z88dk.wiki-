_z88dk-gdb is currently under development_

## Usage

Compile your application with the -debug option:

    zcc +zx -debug main.c -create-app -m

## Running

    ./mame spectrum -window -nomaximize -resolution0 768x576 -debug -debugger gdbstub -debugger_port 1337

    z88dk-gdb -h <connect host> -p <connect port> -x <debug symbols> 