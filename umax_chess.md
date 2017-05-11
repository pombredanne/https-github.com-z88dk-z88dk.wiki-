# CHESS.C

{{libnew:examples:uchess.png?314|uchess}}

[u-Max Chess](http://home.hccnet.nl/h.g.muller/max-src2.html) is touted as one of the smallest chess programs in the world yet despite its size, it manages to pack some sophistication into its implementation.  It was written by H.G.Muller and his webpage dissects the implementation in detail.

The version used here is 1.6; uMax has developed further to version 4.0 but 1.6 is the last version before a large hashmap is incorporated into the implementation.  The hashmap declared is 16MB in size which has no chance of fitting in 64k :P

Because u-Max is aiming to be short, its [source code](http://home.hccnet.nl/h.g.muller/umax1_6.c) appears obfuscated.  You really do have to follow Muller's line-by-line explanation to have any chance at all of following what is going on.

So this program is really a test of the compilers to see if they can parse an obfuscated C program.  When compiling a program originally written for a 32-bit machine, special attention has to be paid to the data types.  Ints are 32-bit on 32-bit machines so, without understanding the range of values that the variables can actually take on within the program, they have to be changed to long type.  Longs that appear on 32-bit machines are likely 64-bit data types and these have no equivalent under z88dk.  Thankfully most 32-bit programs don't use long types.

Before we tackle compilation, a brief note on how to play.  The program will print the chessboard in ascii prior to each move.  To make the computer take the move, just press enter without any input.  Your move is entered in [algebraic notation](https://en.wikipedia.org/wiki/Algebraic_notation_%28chess%29), for example "e2e4".

## sccz80

## sdcc

The [original source code](http://home.hccnet.nl/h.g.muller/umax1_6.c).

Changes made to the source:

   - **All int types are changed to long**.  On 32-bit machines ints are 32-bit.
   - **Function D changed to ANSI declaration**.  D is declared using the old K&R syntax in order to make the program a little bit shorter.  sdcc does not support K&R so the change to ansi must be made.
   - **Iterative deepening loop reduced from 1e6 to 200**.  Reduction to 200 is necessary to get a reasonable response time on a 3.5MHz z80.  Feel free to increase this number to get a better game if your target is faster.
   - **stdio.h is explicitly included**.  If a function is unknown, due to K&R C, it is assumed to return int and take no arguments.  In this program getchar() is relying on this behaviour and the code is relying on the compiler to search its libraries to find this unknown function.  z88dk does not search the libraries for undeclared functions so stdio.h must be included.  Again, this approach was taken to shorten the program.
   - **Pragma added to move stack**.  A pragma is added to move the stack to the top of memory to make a large region available.  Under running conditions, up to 1k of stack space can be used.

**File: "chess.c"**

	:::c
	// zcc +zx -vn -SO3 -clib=sdcc_ix --max-allocs-per-node200000 --reserve-regs-iy chess.c -o chess -lm
	// output is a binary to be executed from 32768
	
	// http://home.hccnet.nl/h.g.muller/max-src2.html
	
	/***************************************************************************/
	/*                               micro-Max,                                */
	/* A chess program smaller than 2KB (of non-blank source), by H.G. Muller  */
	/***************************************************************************/
	/* version 1.6 (1433 non-blank characters) features:                       */
	/* - recursive negamax search                                              */
	/* - quiescence search with recaptures                                     */
	/* - recapture extensions                                                  */
	/* - (internal) iterative deepening                                        */
	/* - best-move-first 'sorting'                                             */
	/* - full FIDE rules and move-legality checking                            */
	
	/* accepts under-promotions: type 1,2,3 (=R,B,N) after input move          */
	/* (input buffer c[] & *P made global, K and N encoding swapped for this)  */
	
	#include `<stdio.h>`
	#pragma output REGISTER_SP = 0
	
	#define W while
	
	//int M=136,S=128,I=8e3,C=799,Q,O,K,N;   
	long M=136,S=128,I=8e3,C=799,Q,O,K,N;
	
	char L,*P,
	w[]={0,1,1,-1,3,3,5,9},                      
	o[]={-16,-15,-17,0,1,16,0,1,16,15,17,0,14,18,31,33,0,
	     7,-1,6,11,8,3,6,                          
	     6,4,5,7,3,5,4,6},                         
	b[129],
	
	n[]=".?+knbrq?*?KNBRQ",
	
	c[9];
	
	//D(k,q,l,e,E,z,n)        
	//int k,q,l,e,E,z,n;      
	long D(long k, long q, long l, long e, long E, long z, long n)
	{                       
	 long j,r,m,v,d,h,i,F,G,s;
	 char t,p,u,x,y,X,Y,H,B;
	
	 q--;                                          
	 d=X=Y=0;                                      
	
	 W(d++<n||
	//   z==8&K==I&&(N<1e6&d<98||
	   z==8&K==I&&(N<200&d<98||                    
	   (K=X,L=Y&~M,d=2)))                          
	 {x=B=X;                                       
	  h=Y&S;                                   
	  m=d>1?-I:e;                                  
	  N++;                                         
	  do{u=b[x];                                   
	   if(u&k)                                     
	   {r=p=u&7;                                   
	    j=o[p+16];                                 
	    W(r=p>2&r<0?-r:-o[++j])                    
	    {A:                                        
	     y=x;F=G=S;                                
	     do{                                       
	      H=y=h?Y^h:y+r;                        
	      if(y&M)break;                            
	      m=E-S&&b[E]&&y-E<2&E-y<2?I:m;    /* castling-on-Pawn-check bug fixed */
	      if(p<3&y==E)H^=16;                       
	      t=b[H];if(t&k|p<3&!(y-x&7)-!t)break;       
	      i=99*w[t&7];                             
	      m=i<0?I:m;                       /* castling-on-Pawn-check bug fixed */
	      if(m>=l)goto C;                          
	
	      if(s=d-(y!=z))                           
	      {v=p<6?b[x+8]-b[y+8]:0;
	       b[G]=b[H]=b[x]=0;b[y]=u|32;             
	       if(!(G&M))b[F]=k+6,v+=30;               
	       if(p<3)                                 
	       {v-=9*((x-2&M||b[x-2]-u)+               
	              (x+2&M||b[x+2]-u)-1);            
	        if(y+r+1&S)b[y]|=7,i+=C;               
	       }
	       v=-D(24-k,-l,m>q?-m:-q,-e-v-i,F,y,s);   
	       if(K-I)                                 
	       {if(v+I&&x==K&y==L&z==8)                
	        {Q=-e-i;O=F;
	         if(b[y]-u&7&&P-c>5)b[y]-=c[4]&3;        /* under-promotions */
	         return l;
	        }v=m;                                   
	       }                                       
	       b[G]=k+6;b[F]=b[y]=0;b[x]=u;b[H]=t;     
	       if(v>m)                         
	        m=v,X=x,Y=y|S&F;                       
	       if(h){h=0;goto A;}                            
	      }
	      if(x+r-y|u&32|                           
	         p>2&(p-3|j-7||                        
	         b[G=x+3^r>>1&7]-k-6                   
	         ||b[G^1]|b[G^2])                      
	        )t+=p<5;                               
	      else F=y;                                
	     }W(!t);                                   
	  }}}W((x=x+9&~M)-B);                          
	C:if(m>I-M|m<M-I)d=98;                         
	  m=m+I?m:-D(24-k,-I,I,0,S,S,1);    
	 }                                             
	 return m+=m<e;                                
	}
	
	main()
	{
	 long k=8;
	
	 K=8;W(K--)
	 {b[K]=(b[K+112]=o[K+24]+8)+8;b[K+16]=18;b[K+96]=9;
	  L=8;W(L--)b[16*L+K+8]=(K-4)*(K-4)+(L-3.5)*(L-3.5); 
	 }                                                   
	
	 W(1)                                                
	 {N=-1;W(++N<121)
	  printf(" %c",N&8&&(N+=7)?10:n[b[N]&15]);
	  P=c;W((*P++=getchar())>10);
	  K=I;                                               
	  if(*c-10)K=*c-16*c[1]+C,L=c[2]-16*c[3]+C;          
	  k^=D(k,-I,I,Q,O,8,2)-I?0:24;
	 }
	}


Following compilation a single binary is produced that should be loaded at address 32768 and executed.

# Conclusion

The final binary size came out to 12303 bytes which includes everything except the stack and character set bitmap.  Fiddling with the library configuration could probably take another 5k off the size so it does manage to come out fairly short, especially considering it is using float math.  Keep in mind this code is standalone, ie it includes the terminal driver code and primitives for drawing on a bitmapped display file; there is no reliance on any existing operating system.

The concept of 'smallest' has changed since the 1980s.  At that time the size of the executable and memory it used was the measure.  1k Chess for the zx81 will likely remain the shortest chess program ever written under that criterion, albeit playing at a far inferior level to uMax Chess.  Today's criterion measures source code size so (mainly in languages other than C) there is plenty of room to make use of giant library routines that perform magic with only a few keystrokes in the source.  That said, uMax Chess is not one of those pretenders and manages to do a heck of a lot with very little.

