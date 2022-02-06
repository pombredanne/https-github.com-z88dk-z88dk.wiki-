_z88dk-gdb is currently under development_

## Usage

Compile your application with the -debug option:

    zcc +zx -debug main.c -create-app -m

## Run On Mame emulator

    ./mame spectrum -window -nomaximize -resolution0 768x576 -debug -debugger gdbstub -debugger_port 1337

## Run on [Fuse fork for Windows](https://github.com/speccytools/fuse/releases/tag/gdbserver)

Start Fuse, select "GDBServer...", select "Enabled"

## Run on [Fuse fork for Mac](https://github.com/speccytools/fuse-for-macosx/releases)

Start Fuse, select "Preferences", select "Debugger", select "Enable GDBServer"

## Run on physical hardware

Install [spectranet-gdbserver](https://github.com/speccytools/spectranet-gdbserver) (requires Spectranet), connect to port 1667

## Connecting

    z88dk-gdb -h <connect host> -p <connect port> -x <debug symbols> 