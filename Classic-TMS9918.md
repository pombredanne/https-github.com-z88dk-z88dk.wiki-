**Note. The screen modes and VDP address mappings changed in October 2019 to match the default MSX settings.**

The TMS9918a/TMS9928a VDP chip is used by the following targets supported by z88dk:

* [Casio PV-2000](Platform---Casio-PV2000)
* [Colecovision Adam](Platform--Colecovision-Adam)
* [Colecovision](Platform--Colecovision)
* [Hanimex Pencil II](Platform---Hanimex-Pencil-II)
* [Memotech MTX](Platform---Memotech-MTX)
* [MSX](Platform---MSX)
* [Nichibutsu My Vision](Platform---Nichibutsu-My-Vision)
* [Spectravideo](Platform---Spectravideo)
* [Sord M5](Platform---Sord-M5)
* [Samsung SPC-1000](Platform-Samsung-SPC-1000)
* [Tatung Einstein](Platform---Tatung-Einstein)

The classic library supplies a library, based on the Hitech-C library by Rafael de Oliveira Jannone. The library provides both high- and low-level access to the VDP chip. The following features are supported:

* ANSI vt100 console using VDP mode 2
* vt52 console using VDP modes 0,1,2
* High resolution graphics (256x192)
* Raster interrupt handler
* Sprite control

## VDP Screen Modes

The VDP screen modes use the following VDP addresses across all targets:

| Mode | Pattern Name | Colour table | Pattern Generator | Sprite Generator | Sprite Attribute |
|-|-|-|-|-|-|
| 0 | $0000 | - | $800 | - | - |
| 1 | $1800 | $2000 | $0000 | $3800 | $1b00 |
| 2 | $1800 | $2000 | $0000 | $3800 | $1b00 |

### Mode 0 (Text 40x24)

Mode 0 is supported by the generic console and can be switched to using `int mode = 1; console_ioctl(IOCTL_GENCON_SET_MODE, &mode);`

The font used is that defined by `-pragma-redirect:CRT_FONT=...`, when the font is changed programmatically using `console_ioctl(IOCTL_GENCON_SET_FONT32,...)` all characters on screen will change. Character codes 32-127 (inclusive) are taken from the specified font, leaving the ranges 0-31 and 128-255 available for your own usage.

In this mode, sprites are not supported. When switching to this mode, the current conio ink/paper is taken as the colour for the whole screen. It is possible to change it by writing to the appropriate VDP register.

### Mode 0 (Text 32x24)

Mode 0 is supported by the generic console and can be switched to using `int mode = 0; console_ioctl(IOCTL_GENCON_SET_MODE, &mode);`

The font used is that defined by `-pragma-redirect:CRT_FONT=...`, it is possible to change the font programmatically using `console_ioctl(IOCTL_GENCON_SET_FONT32,...)` and have multiple fonts displayed on screen.

In this mode, sprites are supported. When switching to this mode, the current conio ink/paper is applied to the entire character set. With the table addresses above it is possible for an application to change them.



### Mode 2 (Text 32x24, Graphics 256x192)

Mode 2 is supported by the generic console and can be switched to using `int mode = 2; console_ioctl(IOCTL_GENCON_SET_MODE, &mode);`

The font used is that defined by `-pragma-redirect:CRT_FONT=...`, when the font is changed programmatically using `console_ioctl(IOCTL_GENCON_SET_FONT32,...)` all characters on screen will change. Character codes 32-127 (inclusive) are taken from the specified font, leaving the ranges 0-31 and 128-255 available for your own usage.

In this mode, sprites are supported. Screen scrolling in this mode is notably slower than in the other modes.

## Raster interrupt

Calling `add_raster_int()` will add an interrupt connected to the VDP interrupt on all platforms apart from the Adam.

# `-lmsxbios` mode

On the MSX and SVI machines, it is possible to use the firmware to drive the VDP.


