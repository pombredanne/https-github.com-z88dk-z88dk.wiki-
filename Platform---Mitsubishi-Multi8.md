# Hardware summary

* Z80 @ 3.993600 Mhz
* 64k RAM, 32k ROM + 2K chargen
* VDP: HD46505 (m6845 clone), 48K VRAM total
* Audio: AY-3-8192 on ports 0x18 and 0x19

## Classic library support (+multi8)

* [x] Native console output
* [x] Native console input
* [ ] ANSI vt100 engine
* [x] Generic console
    * [ ] Redefinable font
    * [ ] UDG support
    * [ ] Paper colour
    * [x] Ink colour
    * [x] Inverse attribute
    * [ ] Bold attribute
    * [ ] Underline attribute
* [ ] Lores graphics
* [x] Hires graphics
* [x] PSG sound
* [ ] One bit sound
* [ ] Inkey driver
* [ ] Hardware joystick
* [ ] File I/O
* [ ] Interrupts
* [ ] RS232

# Compilation

    zcc +multi8 world.c -create-app

The resulting .cas file can be loaded into an emulator by entering the following commands:

    MON
    R
    GC000

The default compile target is limited to about 10k. To be able to write a larger program we need to switch the machine into all ram mode and then load our program. The following is a non-automated way to do it:

    cd z88dk/support/multi8
    zcc +multi8 bootstrap.c -create-app -o bootstrap

This will create our bootstrap loader.

You can then compile your program with the `-subtype=64k` option:

    zcc +multi8 adv_a.c -subtype=64k -create-app -o adv_a

You can then concatenate the two files together:

    cat bootstrap.cas adv_a.cas > adventure.cas

And then load that file as usual.

# Generic console modes

* Mode 0 = 40 column text
* Mode 1 = 80 column text
* Mode 2 = 80 column graphics mode

# Graphics

Use of the graphics library switches to mode 2 and provides a 640x200 monochrome resolution.

# Limitations

Scrolling in mode 2 is particularly slow - we have to move 48kb of data to scroll the display one text line upwards.

# Links

* [Takeda Emulator](http://takeda-toshiya.my.coocan.jp/multi8/)
* [Technical documentation](http://takeda-toshiya.my.coocan.jp/multi8/tech.html)


## Technical notes (not yet implemented)

The im2 vector table is setup at $f000, 8 vectors are available.

A 1Hz interrupt is delivered to the vector at f030 (interrupt 6). 

Setup code:

```
        ld      h,$f7 
        ld      l,$63     
        in      a,($2d)
        and     h
        out     ($2d),a
        ld      a,l
        out     ($2c),a
        ld      de,$f000
        ld      b,8
copy_int_rout:
        push    bc
        ld      hl,int_routine
        ld      bc,8
        ldir
        pop     bc
        djnz    copy_int_rout

        ; f030 is called regularly, set it up as our vector
        ld      a,195
        ld      ($f030),a
        ld      hl,interrupt_dispatch
        ld      ($f031),hl

        EXTERN  im1_vectors
        EXTERN  asm_interrupt_handler

interrupt_dispatch:
        push    af
        push    hl
        ld      hl,im1_vectors
        call    asm_interrupt_handler
        pop     hl
        ld      a,6
ack:
        or      $60
        out     ($2c),a
        pop     af
        ei
        ret

; Interrupt routine that's copied into the vectors
int_routine:
        ei
        reti
        defs      5

```