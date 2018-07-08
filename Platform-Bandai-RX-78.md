# Hardware summary

* SHARP LH0080A @ 4.090909 MHz
* 8k Monitor ROM
* 15k? RAM @0xb000
* 24k? VRAM (6 bit planes, 3 foreground, 3 background)
* Audio: SN76489 on ports 0xff @ 3.579545 MHz
* 24k? ROM cartridges @0x2000

_This target is being brought up, it is not master_

# Graphics

The display on the RX-78 is mirrored to convention, that is bit 0 in a display byte is the left-most pixel displayed. To simplify cross machine compatibility the generic console mirrors bytes so fonts and UDGs don't need to be changed.

The default font used by the generic console is the font that's contained in the IPL rom file. Not all of the characters will print as expected!

# Links

* [Takeda Emulator](http://takeda-toshiya.my.coocan.jp/rx78/index.html)
* [Translated Takeda analysis document](http://interbutt.com/mess/rx78tech.html)