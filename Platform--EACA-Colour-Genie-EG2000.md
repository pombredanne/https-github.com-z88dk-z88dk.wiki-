 # EACA Colour Genie EG2000

The EG2000 was a TRS80 relative, with a different video card.
The monochrome library permits fast drawing working on a single color (RED).
Single bit and PSG sound libraries are supported.


Console disk programs (.CMD files) should be fairly portable between the EG2000 and the TRS80.

## Quick Start

The following command will build a .CMD system file, ready for almost all the existing emulators:

    zcc  +trs80 -lndos -lm -create-app -subtype=eg2000disk program.c

This can then be loaded into Genieous using the `File->Autostart File` menu option and selecting the produced .cmd file.

## [Generic console](Classic-GenericConsole) support

The generic console allows supporting either a custom font (`-pragma-redirect:CRT_FONT=...`) and 32 UDGs, or 128 UDGs. These udgs can be set programmatically using the appropriate ioctl.

You should switch to mode 1 to enable use of the custom font and more than 32 UDGs.


## Links

[The Genieous emulator](http://gaia.atilia.eu/download/Genieous1.0.2_x64.zip)

