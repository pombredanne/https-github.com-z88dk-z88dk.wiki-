### Opus Discovery (`<zxopus.h>`)

Support for the Opus Discovery is built into the zx library.

#### Functions - Joystick

`void set_kempston (int mode)`

Sets the kempston emulation (1=on, 0=off)

`int get_kempston ()`

Gets the kempston emulation status (1=on, 0=off)

#### Functions - Disk

`int opus_getblocks (int drive)`

Gets the number of sectors

`int opus_getblocksize (int drive)`

Gets the sector size

#### Functions - Parallel Port

`void opus_lptwrite (unsigned char databyte)`

Sends a character to parallel port

`unsigned char opus_lptread ()`

Reads a character from the parallel port