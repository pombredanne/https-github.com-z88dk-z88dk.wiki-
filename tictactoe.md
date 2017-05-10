# Silly Tic-Tac-Toe game 

This is a broken algorithm for the Tic Tac Toe game permitting the player to win.
This program has been submitted to the "Minigame Compo 2003" for the 4K category, gaining the position just before the.. last place !


In despite to its little success, it is still a nice example on how to take benefit of assembly language inserts in a C program, to add platform dependent features and squeeze some more power.



##  Portable version 

This is just the base algorithm which can be compared to the extended versions.

`<file>`

/*   TIC TAC TOE  (TRIS)  Game
     By Stefano Bodrato
     ... a broken algorithm so you can win some time   */

#include `<stdio.h>`

char x;

char board[] = {
	'1','2','3',
	'4','5','6',
	'7','8','9'
};


char threes[] = {
	0,1,2,
	3,4,5,
	6,7,8,
	0,4,8,
	6,4,2,
	0,3,6,
	1,4,7,
	2,5,8
};


void printboard()
{

	printf("\n     *     *");
	printf("\n  %c  *  %c  *  %c", board[0], board[1], board[2]);
	printf("\n*****************");
	printf("\n  %c  *  %c  *  %c", board[3], board[4], board[5]);
	printf("\n*****************");
	printf("\n  %c  *  %c  *  %c     ", board[6], board[7], board[8]);
	printf("\n     *     *");
	printf("\n\n");
} 


int domove(char player,char opponent)
{
	
	// Is the center of board free ?
	if ( board[4] != player &&  board[4] != opponent )
	{
		board[4] = player;
		return (0);
	}
	
	
	// Can I win ?
	for (x=0 ; x!=8; x++)
	{
	   if ((board[threes[x*3]]==player && board[threes[x*3+1]]==player) &&
		(board[threes[x*3+2]] != player && board[threes[x*3+2]] != opponent))
	   {
		board[threes[x*3+2]] = player;
		return (1);
	   }
	   else if ((board[threes[x*3+1]]==player && board[threes[x*3+2]]==player) && 
	   	(board[threes[x*3]] != player && board[threes[x*3]] != opponent))
	   {
		board[threes[x*3]] = player;
		return (1);
	   }
	}
	
	
	// Is the opponent going to win ?
	for (x=0 ; x!=8; x++)
	{
	   if ((board[threes[x*3]]==opponent && board[threes[x*3+1]]==opponent) &&
		(board[threes[x*3+2]] != player && board[threes[x*3+2]] != opponent))
	   {
		board[threes[x*3+2]] = player;
		return (0);
	   }
	   else if ((board[threes[x*3+1]]==opponent && board[threes[x*3+2]]==opponent) && 
	   	(board[threes[x*3]] != player && board[threes[x*3]] != opponent))
	   {
		board[threes[x*3]] = player;
		return (0);
	   }
	}
	
	
	// Put in the first free place
	for (x=0 ; x!=9; x++)
	{
		if ( board[x] != player &&  board[x] != opponent )
		{
			board[x] = player;
			return (0);
		}
	}

	return(2);
}


int human(char player,char opponent)
{

char c;
	while (1)
	{
		c=10;
		
		while (c<0 || c>9)
			//c=(getch()-49);
			c=(getk()-49);
		
		if ( board[c] != player &&  board[c] != opponent )
		{
			board[c]=player;
			return (1);
		}
	}
}


int ckwin(char player)
{
	for (x=0 ; x!=8; x++)
	{
	   if ( board[threes[x*3]]==player && 
	   	board[threes[x*3+1]]==player &&
	   	board[threes[x*3+2]]==player) return (1);
	}
	return(0);
}


int ckfree (char player1 , char player2)
{
	for (x=0 ; x!=9; x++)
	{
		if ( board[x] != player1 &&  board[x] != player2 )
		{
			return (1);
		}
	}

	return(0);
}


int ckgame (char player1 , char player2)
{

		if (ckwin (player1))
			{
			  printf ("%c is the winner\n\n",player1);
			  return (1);
			}
		if (ckwin (player2))
			{
			  printf ("%c is the winner\n\n",player2);
			  return (2);
			}
		if (!ckfree (player1,player2))
			{
			  printf ("No winners\n\n");
			  return (3);
			}
		return (0);
}


int main()
{

	//for (x=0 ; x!=9; x++) board[x] = x;
	
	printboard();

	while (ckgame ('X','O') == 0)
	{
		human ('X','O');

		printboard();
		
		if (ckgame ('X','O') != 0)
			return (0);
		
		domove ('O','X');

		printboard();
	}

}

`</file>`

## Commodore 128

The C128 version has a visual effect making the board "glitter" while waiting for the keypress.
It can't be seen on the picture because the monitor has been connected in monochrome mode (luminance only).

Note also that z88dk is permitting to use the Z80 CPU in a Commodore 128 without the need to run CP/M.
The progam has been loaded from a 1541 drive.

{{examples:snippets:tictac128.jpg|}}

`<file>`

/*	     TIC TAC ZAC  (TRIS)  Game
	        C128 inline assembly version
	             for the minigame compo 2003

*/

#include `<stdio.h>`

char x;
char a;

char board[] = {
	'1','2','3',
	'4','5','6',
	'7','8','9'
};


char threes[] = {
	0,1,2,
	3,4,5,
	6,7,8,
	0,4,8,
	6,4,2,
	0,3,6,
	1,4,7,
	2,5,8
};


void printboard()
{

#asm

ld	hl,$200B
ld	a,'*'
ld	b,11
starloop:

ld	de,5
add	hl,de
ld	(hl),a

ld	de,6
add	hl,de
ld	(hl),a

ld	de,40-11
add	hl,de

djnz	starloop


ld	hl,$200B+40*3
ld	d,h
ld	e,l
inc	de
ld	(hl),a
ld	bc,16
ldir


ld	hl,$200B+40*7
ld	d,h
ld	e,l
inc	de
ld	(hl),a
ld	bc,16
ldir

#endasm

a=board[0];

#asm

ld	a,l
ld	hl,$200B+42
ld	(hl),a
#endasm

a=board[1];

#asm

ld	a,l
ld	hl,$200B+48
ld	(hl),a
#endasm

a=board[2];

#asm

ld	a,l
ld	hl,$200B+54
ld	(hl),a
#endasm

a=board[3];

#asm

ld	a,l
ld	hl,$200B+42+40*4
ld	(hl),a
#endasm

a=board[4];

#asm

ld	a,l
ld	hl,$200B+48+40*4
ld	(hl),a
#endasm

a=board[5];

#asm

ld	a,l
ld	hl,$200B+54+40*4
ld	(hl),a
#endasm

a=board[6];

#asm

ld	a,l
ld	hl,$200B+42+40*8
ld	(hl),a
#endasm

a=board[7];

#asm

ld	a,l
ld	hl,$200B+48+40*8
ld	(hl),a
#endasm

a=board[8];

#asm

ld	a,l
ld	hl,$200B+54+40*8
ld	(hl),a
#endasm


/*	printf("\n     *     *");
	printf("\n  %c  *  %c  *  %c", board[0], board[1], board[2]);
	printf("\n*****************");
	printf("\n  %c  *  %c  *  %c", board[3], board[4], board[5]);
	printf("\n*****************");
	printf("\n  %c  *  %c  *  %c     ", board[6], board[7], board[8]);
	printf("\n     *     *");
	printf("\n\n");

*/

} 


int domove(char player,char opponent)
{
	
	// Is the center of board free ?
	if ( board[4] != player &&  board[4] != opponent )
	{
		board[4] = player;
		return (0);
	}
	
	
	// Can I win ?
	for (x=0 ; x!=8; x++)
	{
	   if ((board[threes[x*3]]==player && board[threes[x*3+1]]==player) &&
		(board[threes[x*3+2]] != player && board[threes[x*3+2]] != opponent))
	   {
		board[threes[x*3+2]] = player;
		return (1);
	   }
	   else if ((board[threes[x*3+1]]==player && board[threes[x*3+2]]==player) && 
	   	(board[threes[x*3]] != player && board[threes[x*3]] != opponent))
	   {
		board[threes[x*3]] = player;
		return (1);
	   }
	}
	
	
	// Is the opponent going to win ?
	for (x=0 ; x!=8; x++)
	{
	   if ((board[threes[x*3]]==opponent && board[threes[x*3+1]]==opponent) &&
		(board[threes[x*3+2]] != player && board[threes[x*3+2]] != opponent))
	   {
		board[threes[x*3+2]] = player;
		return (0);
	   }
	   else if ((board[threes[x*3+1]]==opponent && board[threes[x*3+2]]==opponent) && 
	   	(board[threes[x*3]] != player && board[threes[x*3]] != opponent))
	   {
		board[threes[x*3]] = player;
		return (0);
	   }
	}
	
	
	// Put in the first free place
	for (x=0 ; x!=9; x++)
	{
		if ( board[x] != player &&  board[x] != opponent )
		{
			board[x] = player;
			return (0);
		}
	}

	return(2);
}


int human(char player,char opponent)
{

char c;
	while (1)
	{
		c=10;
		
		while (c<0 || c>9)
			//c=(getch()-49);
			c=(getk()-49);
		
		if ( board[c] != player &&  board[c] != opponent )
		{
			board[c]=player;
			return (1);
		}
	}
}


int ckwin(char player)
{
	for (x=0 ; x!=8; x++)
	{
	   if ( board[threes[x*3]]==player && 
	   	board[threes[x*3+1]]==player &&
	   	board[threes[x*3+2]]==player) return (1);
	}
	return(0);
}


int ckfree (char player1 , char player2)
{
	for (x=0 ; x!=9; x++)
	{
		if ( board[x] != player1 &&  board[x] != player2 )
		{
			return (1);
		}
	}

	return(0);
}


int ckgame (char player1 , char player2)
{

		if (ckwin (player1))
			{
			#asm
				ld	hl,yw
				ld	bc,$200d+40*12
				call	printstring
				call	pause
				ld	hl,1
				ret
				
				yw:
				defm "** YOU WIN ** !!!"
				defb 255
			#endasm
			}
		if (ckwin (player2))
			{
			#asm
				ld	hl,cw
				ld	bc,$2007+40*13
				call	printstring
				call	pause
				ld	hl,1
				ret
				
				cw:
				defm "COMMODORE 128 (Z80) WINS !"
				defb 255
			#endasm
			}
		if (!ckfree (player1,player2))
			{
			#asm
				ld	hl,nw
				ld	bc,$2011+40*13
				call	printstring
				call	pause
				ld	hl,1
				ret
				
				nw:
				defm "NO WINNER"
				defb 255
			#endasm
			}
		return (0);
}


int main()
{

#asm

ld	hl,$2000
ld	d,h
ld	e,l
inc	de
ld	a,' '
ld	(hl),a
ld	bc,40*25-1
ldir

ld	hl,$1000
ld	d,h
ld	e,l
inc	de
ld	a,1
ld	(hl),a
ld	bc,40*13-1
ldir

ld	a,3
ld	(hl),a
ld	bc,40*12-1
ldir


	ld	hl,msg
	ld	bc,$2000+40*22
	call	printstring
	
#endasm

	board[0]='1';
	board[1]='2';
	board[2]='3';
	board[3]='4';
	board[4]='5';
	board[5]='6';
	board[6]='7';
	board[7]='8';
	board[8]='9';

	//for (x=0 ; x!=9; x++) board[x] = x;
	
	printboard();

	while (ckgame ('X','O') == 0)
	{
		human ('X','O');

		printboard();
		
		if (ckgame ('X','O') != 0)
			return (0);
		
		domove ('O','X');

		printboard();
	}

}


int getk ()
{
	
#asm

; wait for tick on timer to sync the colour effect
ld	bc,56324
xor	a
out	(c),a
.dsync
in	a,(c)
and	a
jr	z,dsync


; now paint the grid
ld	hl,$100B
ld	a,(curclr)
inc	a
ld	(curclr),a
ld	b,11

starclr:

ld	de,5
add	hl,de
ld	(hl),a

ld	de,6
add	hl,de
ld	(hl),a

ld	de,40-11
add	hl,de

djnz	starclr

ld	hl,$100B+40*3
ld	d,h
ld	e,l
inc	de
ld	(hl),a
ld	bc,16
ldir


ld	hl,$100B+40*7
ld	d,h
ld	e,l
inc	de
ld	(hl),a
ld	bc,16
ldir


; Now read the keyboard
	ld	hl,keytab
	ld	de,8
	ld	a,@01111111
	ld	bc,$dc00
.kloop1
	out	(c),a
	ld	e,a
	inc	bc
	in	a,(c)
	dec	bc
	cp	255
	jr	nz,scanline
	ld	a,e
	scf
	rra
	jr	nc,nokey
	ld	e,8
	add	hl,de
	jr	kloop1
.scanline
	rla
	jr	nc,readtab
	inc	hl
	jr	scanline
.readtab
	ld	a,(hl)
	jr	done

.nokey
	xor	a

.done
	ld	h,0
	ld	l,a
	ret

.curclr	defb	0

.keytab
defb	  3,'Q','c',' ','2','c','a','1'
defb	'/','^','=','r','h',';','*','_'
defb	',','@',':','.','-','L','P','+'
defb	'N','O','K','M','0','J','I','9'
defb	'V','U','H','B','8','G','Y','7'
defb	'X','T','F','C','6','D','R','5'
defb	'l','E','S','Z','4','A','W','3'
defb	'_','5','3','1','7','_', 13, 12

#endasm

}

#asm

printstring:
			ld	a,(hl)
			cp	255
			ret	z
			ld	(bc),a
			inc	hl
			inc	bc
			jr printstring
			ret

pause:
			call getk
			ld	a,l
			and	a
			jr	nz,pause
wloop:
			call getk
			ld	a,l
			and	a
			jr	z,wloop
			ret

	msg:
	defm "BY STEFANO BODRATO - HTTP://Z88DK.SF.NET"
	defb 255

#endasm

`</file>`

## ZX 81

The ZX 81 version has been written long time before the latest fixes. The assembler portions permitted the program to run flicker-free. 

`<file>`

/*
	     TIC TAC ZAC  (TRIS)  Game
	        ZX81 inline assembly version
	             for the minigame compo 2003

 compile with:  zcc +zx81 -startup=2 tic81.c
 the "startup=2" parameters forces the program to run in SLOW mode
 
 By Stefano Bodrato, 18 September 2003

*/

/* Shouldn't be necessary, but I put it in anyway */
#pragma output nostreams

char x;
char a;

char board[9];


char threes[] = {
	0,1,2,
	3,4,5,
	6,7,8,
	0,4,8,
	6,4,2,
	0,3,6,
	1,4,7,
	2,5,8
};


void printline()
{
#asm

ld	a,$76	; NewLine
rst	16

ld	b,8
sl1a:
ld	a,0	; SPACE
rst	16
djnz	sl1a

ld	b,17
lloop:
ld	a,8
rst	16
djnz	lloop
#endasm

}

void printspline()
{
#asm

ld	a,$76	; NewLine
rst	16


ld	b,8
sl1b:
ld	a,0	; SPACE
rst	16
djnz	sl1b

ld	b,2
rl1:
push	bc

ld	b,5
sl1:
ld	a,0	; SPACE
rst	16
djnz	sl1

ld	a,8
rst	16
pop	bc
djnz	rl1

#endasm

}

void printboard()
{
#asm

;call $0A2A	;CLS (flickers)
ld	bc,0	; PRINT AT 0,0;  (better !!)
call $8f5
#endasm

	// A smaller board

printspline();

#asm

ld	a,$76	;NEWLINE
rst	16

ld	b,10
sl1c:
ld	a,0	;SPACE
rst	16
djnz	sl1c
#endasm

a=board[0];

#asm

call	putpiece
#endasm

a=board[1];

#asm

call	putpiece
#endasm

a=board[2];

#asm

ld	a,l
rst	16
#endasm

printspline();
printline();
printspline();

#asm

ld	a,$76
rst	16

ld	b,10
sl1d:
ld	a,0
rst	16
djnz	sl1d

#endasm

a=board[3];

#asm

call	putpiece
#endasm

a=board[4];

#asm

call	putpiece
#endasm

a=board[5];

#asm

ld	a,l
rst	16
#endasm

printspline();
printline();
printspline();

#asm

ld	a,$76
rst	16

ld	b,10
sl1e:
ld	a,0
rst	16
djnz	sl1e

#endasm

a=board[6];

#asm

call	putpiece
#endasm

a=board[7];

#asm

call	putpiece
#endasm

a=board[8];

#asm

ld	a,l
rst	16
#endasm

printspline();

#asm

ld	a,$76
rst	16
#endasm

/*
	printf("\n     *     *");
	printf("\n  %c  *  %c  *  %c", board[0], board[1], board[2]);
	printf("\n*****************");
	printf("\n  %c  *  %c  *  %c", board[3], board[4], board[5]);
	printf("\n*****************");
	printf("\n  %c  *  %c  *  %c", board[6], board[7], board[8]);
	printf("\n     *     *");
	printf("\n\n");

*/
} 

#asm

putpiece:
ld	a,l
rst	16
ld	a,0
push	af
rst	16
pop	af
rst	16
ld	a,8
rst	16
ld	a,0
push	af
rst	16
pop	af
rst	16
ret
#endasm


int domove(char player,char opponent)
{
	
	// Is the center of board free ?
	if ( board[4] != player &&  board[4] != opponent )
	{
		board[4] = player;
		return (0);
	}
	
	
	// Can I win ?
	for (x=0 ; x!=8; x++)
	{
	   if ((board[threes[x*3]]==player && board[threes[x*3+1]]==player) &&
		(board[threes[x*3+2]] != player && board[threes[x*3+2]] != opponent))
	   {
		board[threes[x*3+2]] = player;
		return (1);
	   }
	   else if ((board[threes[x*3+1]]==player && board[threes[x*3+2]]==player) && 
	   	(board[threes[x*3]] != player && board[threes[x*3]] != opponent))
	   {
		board[threes[x*3]] = player;
		return (1);
	   }
	}
	
	
	// Is the opponent going to win ?
	for (x=0 ; x!=8; x++)
	{
	   if ((board[threes[x*3]]==opponent && board[threes[x*3+1]]==opponent) &&
		(board[threes[x*3+2]] != player && board[threes[x*3+2]] != opponent))
	   {
		board[threes[x*3+2]] = player;
		return (0);
	   }
	   else if ((board[threes[x*3+1]]==opponent && board[threes[x*3+2]]==opponent) && 
	   	(board[threes[x*3]] != player && board[threes[x*3]] != opponent))
	   {
		board[threes[x*3]] = player;
		return (0);
	   }
	}
	
	
	// Put in the first free place
	for (x=0 ; x!=9; x++)
	{
		if ( board[x] != player &&  board[x] != opponent )
		{
			board[x] = player;
			return (0);
		}
	}

	return(2);
}


int human(char player,char opponent)
{

char c;
	while (1)
	{
		c=10;
		
		while (c<0 || c>9)
			//c=(getch()-49);
			c=(getk()-49);
		
		if ( board[c] != player &&  board[c] != opponent )
		{
			board[c]=player;
			return (1);
		}
	}
}


int ckwin(char player)
{
	for (x=0 ; x!=8; x++)
	{
	   if ( board[threes[x*3]]==player && 
	   	board[threes[x*3+1]]==player &&
	   	board[threes[x*3+2]]==player) return (1);
	}
	return(0);
}


int ckfree (char player1 , char player2)
{
	for (x=0 ; x!=9; x++)
	{
		if ( board[x] != player1 &&  board[x] != player2 )
		{
			return (1);
		}
	}

	return(0);
}


int ckgame (char player1 , char player2)
{

		if (ckwin (player1))
			{
			#asm
				ld	hl,yw
				call	printmsg
				ld	hl,1
				ret
				yw:	defb $3e,$34,$3a,$16
					defb $3c,$2e,$33,0
			#endasm
			  //printf ("%c is the winner\n\n",player1);
			  //return (1);
			}
		if (ckwin (player2))
			{
			#asm
				ld	hl,iw
				call	printmsg
				ld	hl,2
				ret
				iw:	defb $3f,$3d,$16
					defb $3c,$2e,$33,$38,0
			#endasm
			  //printf ("%c is the winner\n\n",player2);
			  //return (2);
			}
		if (!ckfree (player1,player2))
			{
			#asm
				
				ld	hl,nw
				call	printmsg
				ld	hl,1
				ret
				nw:	defb $33,$34,$16
					defb $3c,$2e,$33,$33,$2a,$37,$38,0
			#endasm
			 // printf ("No winners\n\n");
			 // return (3);
			}
		return (0);
}


int main()
{

	//for (x=0 ; x!=9; x++) board[x] = x;
	board[0]=0x1d;
	board[1]=0x1e;
	board[2]=0x1f;
	board[3]=0x20;
	board[4]=0x21;
	board[5]=0x22;
	board[6]=0x23;
	board[7]=0x24;
	board[8]=0x25;

	printboard();

	while (ckgame (0x3d,0x34) == 0)
	{
		human (0x3d,0x34);

		printboard();
		
		if (ckgame (0x3d,0x34) != 0)
			return (0);
		
		domove (0x34,0x3d);
		
/*	while (ckgame ('X','O') == 0)
	{
		human ('X','O');

		printboard();
		
		if (ckgame ('X','O') != 0)
			return (0);
		
		domove ('O','X');

*/
		printboard();
	}

}


int getk()
{
#asm

.isntchar
	call	699
	ld	a,h
	add	a,2
	jr	c,isntchar
	ld	b,h
	ld	c,l
	call	1981
	ld	a,(hl)
	add	20
	ld	h,0
	ld	l,a
	ret
#endasm

}


#asm

printmsg:
ld	a,(hl)
and	a
ret	z
push	hl
rst	16
pop	hl
inc	hl
jr	printmsg

#endasm

`</file>`


## Links

[Minigame Compo 2003 (see position 62)](http://starbase.globalpc.net/minigame/index.results.html)


