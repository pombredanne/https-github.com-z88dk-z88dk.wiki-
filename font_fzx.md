# FONT/FZX.H

| | |
|-|-|
| Include    | #include `<font/fzx.h>`                                                                                              |
| Header     | [{z88dk}/include/_DEVELOPMENT/sccz80/font/fzx.h](https://raw.githubusercontent.com/z88dk/z88dk/master/include/_DEVELOPMENT/sccz80/font/fzx.h?content-type=text%2Fplain) |
| | [{z88dk}/include/_DEVELOPMENT/sdcc/font/fzx.h](https://raw.githubusercontent.com/z88dk/z88dk/master/include/_DEVELOPMENT/sdcc/font/fzx.h?content-type=text%2Fplain) |
| Source     | [{z88dk}/libsrc/_DEVELOPMENT/font/fzx](https://raw.githubusercontent.com/z88dk/z88dk/master/libsrc/_DEVELOPMENT/font/fzx/)                     |


[fonts list](fonts list)


# EXTENT

## uint16_t fzx_buffer_extent(struct fzx_font *ff, char *buf, uint16_t buflen)

## char *fzx_buffer_partition(struct fzx_font *ff, char *buf, uint16_t buflen, uint16_t allowed_width)

## char *fzx_buffer_partition_ww(struct fzx_font *ff, char *buf, uint16_t buflen, uint16_t allowed_width)

## uint16_t fzx_string_extent(struct fzx_font *ff, char *s)

## char *fzx_string_partition(struct fzx_font *ff, char *s, uint16_t allowed_width)

## char *fzx_string_partition_ww(struct fzx_font *ff, char *s, uint16_t allowed_width)


# RENDER

## int fzx_putc(struct fzx_state *fs, int c)

## int fzx_puts(struct fzx_state *fs, char *s)

## int fzx_puts_justified(struct fzx_state *fs, char *s, uint16_t allowed_width)

## int fzx_write(struct fzx_state *fs, char *buf, uint16_t buflen)

## int fzx_write_justified(struct fzx_state *fs, char *buf, uint16_t buflen, uint16_t allowed_width)


# RENDER MODES

## void _fzx_draw_or(void)

## void _fzx_draw_reset(void)

## void _fzx_draw_xor(void)


# MISCELLANEOUS

## void fzx_at(struct fzx_state *fs, uint16_t x, uint16_t y)

## char *fzx_char_metrics(struct fzx_font *ff, struct fzx_cmetric *fm, int c)

## uint16_t fzx_glyph_width(struct fzx_font *ff, int c)

## void fzx_state_init(struct fzx_state *fs, struct fzx_font *ff, struct r_Rect16 *window)


