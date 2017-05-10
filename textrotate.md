
{{:examples:snippets:zxspectrum:textrotate.gif|}}


To build it for a Spectrum:
    zcc +zx -create-app -lndos -lm rotate.c
- or -
    zcc +zx -create-app -lndos -lmzx_tiny rotate.c


To build it for a ZX81:
    zcc +zx81 -startup=3 -lgfx81hr192 -create-app -lm rotate.c



	
	
	
	#include `<stdio.h>`
	#include `<math.h>`
	#include `<graphics.h>`
	#include `<string.h>`
	
	// Set the font location here.
	
	#ifdef __SPECTRUM__
	#define FONT 15360
	#endif
	
	#ifdef __ZX81__
	#define FONT 7680
	#endif
	
	
	float s,g,e;
	int a,b,c,d;
	unsigned char p,q;
	
	void rotate_text(int width, int height, int x, int y, int angle, char *text)
	{
	 x+=width*7; y+=height*8;
	 g=angle/180.0*pi();
	 s=sin(g); g=cos(g);
	 a=0;
	 
	#ifdef __ZX81__
	 text=strlwr(text);
	#endif
	 
	 while (text[a]!=0) {
	#ifdef __ZX81__
	   b=FONT+ascii_zx(text[a])*8+7;
	#else
	   b=FONT+text[a]*8+7;
	#endif
	   for (c=6;c>0;c--) {
	     p=*(b-c);
	     for (d=0;d<7;d++) {
	       q=p/2;
	       if (p&1)
	         for (e=0.0;e<(0.0+height);e+=0.4) {
	           plot(x+(c*height+e)*s-d*width*g,y-(c*height+e)*g-d*width*s);
	           drawr((1.2-width)*g,(1.2-width)*s);
	         }
	       p=q;
	     }
	   }
	   x=x+g*width*8.0;
	   y=y+s*width*8.0-g*width;
	   
	   a++;
	 }
	}
	
	
	void main()
	{
		clg();
		rotate_text(3, 5, 0, 0, 38, "Hello World!");
	}
	


