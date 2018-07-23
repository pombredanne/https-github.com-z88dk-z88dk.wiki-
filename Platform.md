 | Machine | Native Console I/O | VT100 Console I/O | [Generic Console](Classic-GenericConsole) | [Monochrome Graphics](library/monographics) | [File I/O](library/stdio) | [Sound](library/sound) | Other features / notes | 
 |---|---|---|---|---|---|---|---|
[ABC80](Platform---Luxor-ABC80) | Yes | 40x24 | 40x24 | 78x72 | No | No | 
[ABC800](Platform---Luxor-ABC800)| Yes | No |No | No | No | No | Untested |
[Alphatronic PC](Platform---Alphatronic-PC)| No | No | 40x24 | 80x72 | No | PSG |
[Amstrad CPC](Platform---Amstrad-CPC)| Yes | Yes | Yes | 640x200[+graylib](library/graylib)| Yes | No | CPCRSlib partially imported (no tiles) |
[Amstrad NC100/NC150](Platform---Amstrad-NC) | Yes | No | No | 480x64| No | No | | 
[Amstrad NC200](Platform---Amstrad-NC) | Yes | No | No | 480x128 | No | No | | 
[Mattel Aquarius](Platform---Mattel-Aquarius)| Yes | 40x24| 40x24 | 80x72 | No | 1 bit +PSG | | 
[Bandai RX-78](Platform-Bandai-RX-78) | Yes | No | 24x23 | 192x184 | No | PSG |
[Knight 2000 (Aussie Byte)](Platform---Aussie) | Yes | 80x25| No | not yet | No | 1 bit| Untested and incomplete graphics lib| 
[Canon X-07](Platform---Canon-X07) | Yes | No | No | No | No | No | Initial stage | 
[Casio FP-1100](Platform---Casio-FP1000) | No | No | 40x25,80x25 | No | No | No |
[Casio PV-1000](Platform---Casio-PV1000) | No | No | 28x24 | 56x48 | No | No |
[Casio PV-2000](Platform---Casio-PV2000) | No | No | 32x24 | 256x192 | No | PSG |
[Cambridge Z88](Platform---Cambridge-Z88)| Yes | Yes | No | 256x64| Yes | 1 bit| Far memory support, ZSock + many other features | 
[Colecovision](Platform--Colecovision)| No | No | 32x24 | 256x192 | No | PSG |
[Commodore 128 (z80)](Platform--Commodore-c128)| No | 40x25| No | 80x50, 640x200, 640x480 | CP/M only | SID + PSG + 1 bit| [Steve Goldsmith tools](library/c128/sgtools) are supported | 
[CCE MC-1000](Platform---CCE-MC1000) | Yes | [24..85x24](Platform---Sinclair-ZX-Spectrum#the-vtansi-console-driver)| 32x24 (hires)/32x16 | 256×192| No | 1 bit| | 
[CP/M](Platform---CPM) | OS calls | If hardware supported| No | Target specific | Yes | No | | 
[S-OS (The Sentinel)](Platform---SOS)| OS calls | No | No | No |  Yes | No | Multi platform OS published in a Japanese PC magazine | 
[EACA EG2000](Platform--EACA-Colour-Genie-EG2000)| 40x24..25| No | 40x24 - default |  160x96..102 | No | 1 bit +PSG | Sound output via cassette port| 
[Epson PX-4/HC-40](Platform---Epson-PX4)| 40x8 | [30..80x8](Platform---Epson-PX4#the-vtansi-console-driver)| No | 240x64| No | No | | 
[Epson PX-8/HC-80](Platform---Epson-PX8) | 80x8 | No | No | 480x64* | No | No | *GFX via CONOUT driver, no screen reading (POINT, XOR missing)| 
[Tatung Einstein](Platform---Tatung-Einstein)| 40x24| [24..85x24](Platform---MSX#the-vtansi-console-driver) (Colour) | No | 256x192 | Yes | No | Extension library to CP/M.Most of GFXLIB by Rafael de Oliveira Jannone| 
[Enterprise 64/128](Platform---Enterprise64) | 40x25| No | No | 336x243*, 672x243*| No | Yes | *GFX via EXOS | 
[Galaksija](Platform---Galaksija)| 32x16 (B&W)| 32x16 (B&W) | No | 64x48 | No | 1 bit* | (via tape output) | 
[Jupiter Ace](Platform---Jupiter-Ace)| 32x24| 32x24 (B&W)| 32x24 - default| 64x48, 64x72| No | 1 bit| | 
[Kaypro](Platform---Kaypro) | 80x25 (ADM3) | No | No | 160x100('84) 80x50('83) | No | No | Extension library to the CP/M base| 
[Lambda 8300](Platform---Lambda) | 32x24 (B&W)| 32x24 (TXT)| No | 64x48 |  No | 1 bit* | (via tape output) | 
[Osborne 1](Platform---Osborne)| 52x24 (*) | No | No | 104x48| No | No | Extension library to the CP/M base (* an official HW mod permitted higher resolutions)| 
[Otrona Attachè](Platform---Otrona) | 80x25 (ADM3) | No | No |  320x240 | No | No | Extension library to the CP/M base| 
[Camputers Lynx](Platform---Camputers-Lynx)| Yes | No | No | No | No | No | Support is at an initial stage| 
[MSX](Platform---MSX)| 40x24| [24..85x24](Platform---MSX#the-vtansi-console-driver) (Colour)| 32x24 |  256x192| No | 1 bit +PSG | GFXLIB by Rafael de Oliveira Jannone| 
[Memotech MTX](Platform---Memotech-MTX)| 40x24| [24..85x24](Platform---MSX#the-vtansi-console-driver) (Colour)| 32x24 | 256x192 | No | PSG (+ 1 bit)  | Most of GFXLIB by Rafael de Oliveira Jannone| 
[MicroBee](Platform---Microbee)| Yes | 40x25, 80x25 | No | 640x275, 512x256, 320x275, 160x75 | No | 1 bit| Native console: 64x16 when in RUNM mode, 80x24 when used as an extension library to the CP/M base | 
[Mitsubishi Multi8](Platform---Mitsubishi-Multi8)| Yes | No | 40x25, 80x25 | 640x200 | PSG | No |
[Nascom](Platform---Nascom)| 48x16| 48x16| 48x16 - default| 96x48 | No | No | | 
[Grundy Newbrain](Platform---Grundy-Newbrain)| Yes | No | No |  No* | No* | No | * could work on expanded systems in non-standard mode, via [stream functions](library/newbrain#streams) | 
[v6z80p (OSCA)](Platform---OSCA) | 40x24| 40x24| No | 320x200 | Yes | No | 2 different file access libraries | 
[Pac Man HW](Platform---Pacman)| 28x36| No | No |  56x72 | No | No lib | | 
[PC-6001](Platform---NEC-PC6001) | 32x16, 32x24| 32x16 (*)|  32x16, 32x24 | 64x48, 128x192, 256x192 | No | PSG only | | 
[Philips P2000](Platform---Philips-P2000)| 40x24| Monochrome 40x24 | No | 78×72 | No | 1 bit| | 
[Philips C7420](Platform---Philips-C7420)| 39x20| No | No | Not Yet | No | No | | 
[Philips VG-5000](Platform----Philips-VG5000)| 40x24| 40x24| 40x24 | 80×72 | No | 1 bit| | 
[Rabbit Control Module](Platform---Rabbit) ||| | || |  |
[Robotron Z1013](Platform---Robotron-Z1013)| 32x32| 32x32| 32x32 - default|  64x64 | No | No | | 
[Robotron Z9001, KC85/1, KC87](Platform---Robotron-Z9001)| 40x24| 40x24| 40x24 | 80x48, 320x192| No | 1 bit| Model variants: KC85/1, KC87| 
[(Robotron) VEB Mikroelektronik HC-900, KC85/2..KC85/5](Platform---KC) | 40x24| No | No | 320x256 | No | No | Model variants: HC-900 KC85/2..KC85/5 | 
[Sam Coupe](Platform---Sam-Coupe)| 32x24| [24..85x24](Platform---Sinclair-ZX-Spectrum#the-vtansi-console-driver) (Colour) | No | No | No | 1 bit| Sprite Pack. Music might play at a higher pitch due to CPU frequency. | 
[Samsung SPC-1000](Platform-Samsung-SPC-1000)| 32x16 | No | 32x16 + 32x24 | 64x32 + 256x192 | No | Yes | VDP extension is supported
[Sega Master System](Platform---SMS) |32x24|| | || PSG (+ 1 bit) | |
[Sega SC-3000/Sega SG-1000](Platform---Sega-SC3000) | 40x24| [24..85x24](Platform---MSX#the-vtansi-console-driver) (Colour) | 32x24 | 256x192 | No | PSG (+ 1 bit)  | Most of GFXLIB by Rafael de Oliveira Jannone | 
[Sharp PC-G8xx, PC-E2xx](Platform---Sharp-PC)| No | 24x4 (24x6)|  No | No | No | No | | 
[Sharp MZ (80,700,800)](Platform---Sharp-MZ) | 40x25| 40x25| 40x25 | 80x50 | No | PSG (+ 1 bit)  | Many appmake extras | 
[Sharp OZ](Platform---Sharp-OZ700) | Yes | No | No | 239x80| No | No | Experimental| 
[Sharp MZ2500](Platform---Sharp-MZ2500) | 40(80)x25 | No| No | No | No | No  | Initial support, max 4kb | 
[Sharp X1](Platform---Sharp-X1)| No | 40(80)x25| No | No | No | PSG only | | 
[Exidy Sorcerer](Platform---Sorcerer)| 64x30| No | 64x30 | 128x60| No | No | '--300bps' extra mode in appmake| 
[SORD M5](Platform---Sord-M5)| 32x24| [24..85x24](Platform---MSX#the-vtansi-console-driver) (Colour) | 32x24 | 256x192 | No | PSG (+ 1 bit) | Most of GFXLIB by Rafael de Oliveira Jannone | 
[Spectravideo SVI](Platform---Spectravideo)| 40x24| [24..85x24](Platform---MSX#the-vtansi-console-driver) (Colour)| 32x24 | 256x192 | No | 1 bit +PSG | GFXLIB by Rafael de Oliveira Jannone| 
[Sprinter](Platform---Peters-Plus-Sprinter)| 80x35 | 80x35 (Colour)| 80x35 -default | No | Yes | No | Experimental port. Developed under the SPRINT emulator| 
[TI82](Platform---TI-Calculators)| 16x8 | 32x8 (B&W) | No | 96x64[+graylib](library/graylib)| No | 1 bit| Grey graphics run on the VTI emulator but problems have been reported with the real hardware| 
[TI83](Platform---TI-Calculators)| 16x8 | 32x8 (B&W) | No | 96x64[+graylib](library/graylib)| No | 1 bit| Sound output via infrared port| 
[ TI83+](Platform---TI-Calculators)| 16x8 | 32x8 (B&W) | No | 128x64 [+graylib](library/graylib)| No | 1 bit| Sound output via infrared port|
[TI85](Platform---TI-Calculators)| 21x8 | 32x8 (B&W) | No | 128x64| No | 1 bit| Sound output via infrared port| 
[TI86](Platform---TI-Calculators)| 21x8 | 32x8 (B&W) | No | 128x64| No | 1 bit| Sound output via infrared port| 
[TIKI-100](Platform---Tiki100) | Yes | No | No | 1024x256| No | PSG only | Extension library to the CP/M base| 
[TRS-80](Platform---TRS80) | 64x16 | No | 64x16 - default| 128x48| No | 1 bit| Sound output via cassette port| 
[TS2068](Platform---Timex-TS2068)| [32x24, 64x24, 128x24](Platform---Sinclair-ZX-Spectrum#the-standard-zx-spectrum-console-driver) | [24..85x24](Platform---Sinclair-ZX-Spectrum#the-vtansi-console-driver) (Colour) | 32x24,64x24, 128x24* | 256x192 | No | 1 bit| Sprite Pack | 
[VZ/Laser 200](Platform---VZ200) | 32x16| 32x12 (B&W)| 32x16 | 128x64 + 64x32| No | 1 bit| | 
[Xircom Rex 6000](Platform---Xircom-REX6000) | No | No | No | No | No | No | Graham Cobb's stdio library was due to be integrated into z88dk v1.6. The Rex supplies has its own library mostly divergent from the standard z88dk library | 
[ZX80](Platform---Sinclair-ZX80) | 32x24| 32x24 (TXT)| 32x24 - default| 64x48[+graylib](library/graylib)| No | as for ZX81| Good compatibility with ZX81, tricks to try keeping the display stable| 
[ZX81](Platform---Sinclair-ZX81) | 32x24| 32x24 (TXT), [24..85x24](Platform---Sinclair-ZX81#the-vtansi-console-driver) (HRG) | 32x24 -default | 64x48, 64x72, +HRG modes [+graylib](library/graylib)| No | 1 bit + PSG| Sprite Pack, 1bit sound blanks the screen | 
[ZX Spectrum](Platform---Sinclair-ZX-Spectrum) | [32x24 and 64x24](Platform---Sinclair-ZX-Spectrum#the-standard-zx-spectrum-console-driver) | [24..85x24](Platform---Sinclair-ZX-Spectrum#the-vtansi-console-driver) (Colour) | 32x24 and 64x24 - default| 256x192 | Yes | 1 bit + PSG| Sprite Pack. Native file IO available for +3 and Microdrive; BASIC driver supports many other disc types. | 
ZXVGS| -| No | - | - | Yes | -| Not entirely integrated since we're not sure how to classify it!| 