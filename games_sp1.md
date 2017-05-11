# SP1.H

 | Include    | #include `<games/sp1.h>`                                                                                              |                              
 | ----------------------------------------------------------------------------------------------------------------------------------                              
 | Header     | [{z88dk}/include/_DEVELOPMENT/sccz80/games/sp1.h](https://raw.githubusercontent.com/z88dk/z88dk/master/include/_DEVELOPMENT/sccz80/games/sp1.h) |   
 | | [{z88dk}/include/_DEVELOPMENT/sccz80/games/sp1/sp1_zx.h](https://raw.githubusercontent.com/z88dk/z88dk/master/include/_DEVELOPMENT/sccz80/games/sp1/sp1_zx.h) |
 | | [{z88dk}/include/_DEVELOPMENT/sdcc/games/sp1.h](https://raw.githubusercontent.com/z88dk/z88dk/master/include/_DEVELOPMENT/sdcc/games/sp1.h) |                  
 | | [{z88dk}/include/_DEVELOPMENT/sdcc/games/sp1/sp1_zx.h](https://raw.githubusercontent.com/z88dk/z88dk/master/include/_DEVELOPMENT/sdcc/games/sp1/sp1_zx.h) |    
 | Source     | [{z88dk}/libsrc/_DEVELOPMENT/temp/sp1](https://raw.githubusercontent.com/z88dk/z88dk/master/libsrc/_DEVELOPMENT/temp/sp1)                     |     

# UPDATER

## void sp1_Initialize(uint16_t iflag, uint16_t colour, uint16_t tile)

## void sp1_UpdateNow(void)

## struct sp1_update *sp1_GetUpdateStruct(uint16_t row, uint16_t col)

## void sp1_IterateUpdateArr(struct sp1_update **ua, void (*hook)(struct sp1_update *))

## void sp1_IterateUpdateRect(struct sp1_Rect *r, void (*hook)(struct sp1_update *))

## void sp1_InvUpdateStruct(struct sp1_update *u)

## void sp1_ValUpdateStruct(struct sp1_update *u)

## void sp1_DrawUpdateStructIfInv(struct sp1_update *u)

## void sp1_DrawUpdateStructIfVal(struct sp1_update *u)

## void sp1_DrawUpdateStructIfNotRem(struct sp1_update *u)

## void sp1_DrawUpdateStructAlways(struct sp1_update *u)

## void sp1_RemoveUpdateStruct(struct sp1_update *u)

## void sp1_RestoreUpdateStruct(struct sp1_update *u)

## void sp1_Invalidate(struct sp1_Rect *r)

## void sp1_Validate(struct sp1_Rect *r)

# TILES

## void *sp1_TileEntry(uint16_t c, void *def)

## void sp1_PrintAt(uint16_t row, uint16_t col, uint16_t colour, uint16_t tile)

## void sp1_PrintAtInv(uint16_t row, uint16_t col, uint16_t colour, uint16_t tile)

## uint16_t sp1_ScreenStr(uint16_t row, uint16_t col)

## uint16_t sp1_ScreenAttr(uint16_t row, uint16_t col)

## void sp1_PrintString(struct sp1_pss *ps, uint8_t *s)

## void sp1_SetPrintPos(struct sp1_pss *ps, uint16_t row, uint16_t col)

## void sp1_GetTiles(struct sp1_Rect *r, struct sp1_tp *dest)

## void sp1_PutTiles(struct sp1_Rect *r, struct sp1_tp *src)

## void sp1_PutTilesInv(struct sp1_Rect *r, struct sp1_tp *src)

## void sp1_ClearRect(struct sp1_Rect *r, uint16_t colour, uint16_t tile, uint16_t rflag)

## void sp1_ClearRectInv(struct sp1_Rect *r, uint16_t colour, uint16_t tile, uint16_t rflag)

# SPRITES

## struct sp1_ss *sp1_CreateSpr(void (*drawf)(void), uint16_t type, uint16_t height, int graphic, uint16_t plane)

## uint16_t sp1_AddColSpr(struct sp1_ss *s, void (*drawf)(void), uint16_t type, int graphic, uint16_t plane)

## void sp1_ChangeSprType(struct sp1_cs *c,void (*drawf)(void))

## void sp1_DeleteSpr(struct sp1_ss *s)

## void sp1_MoveSprAbs(struct sp1_ss *s, struct sp1_Rect *clip, void *frame, uint16_t row, uint16_t col, uint16_t vrot, uint16_t hrot)

## void sp1_MoveSprRel(struct sp1_ss *s, struct sp1_Rect *clip, void *frame, int rel_row, int rel_col, int rel_vrot, int rel_hrot)

## void sp1_MoveSprPix(struct sp1_ss *s, struct sp1_Rect *clip, void *frame, uint16_t x, uint16_t y)

## void sp1_IterateSprChar(struct sp1_ss *s,void (*hook)(struct sp1_cs *))

## void sp1_IterateUpdateSpr(struct sp1_ss *s, void (*hook)(struct sp1_update *))

## void sp1_GetSprClrAddr(struct sp1_ss *s, uint8_t **sprdest)

## void sp1_PutSprClr(uint8_t **sprdest, struct sp1_ap *src, uint16_t n)

## void sp1_GetSprClr(uint8_t **sprsrc, struct sp1_ap *dest, uint16_t n)

## void *sp1_PreShiftSpr(uint16_t flag, uint16_t height, uint16_t width, void *srcframe, void *destframe, uint16_t rshift)

## void sp1_InitCharStruct(struct sp1_cs *cs, void (*drawf)(void), uint16_t type, void *graphic, uint16_t plane)

## void sp1_InsertCharStruct(struct sp1_update *u, struct sp1_cs *cs)

## void sp1_RemoveCharStruct(struct sp1_cs *cs)

