# MATHS LIBRARY (math.h, float.h, m.lib)

 | Header     | [{z88dk}/include/math.h](https///raw.githubusercontent.com/z88dk/z88dk/master/include/math.h), [{z88dk}/include/float.h](https///raw.githubusercontent.com/z88dk/z88dk/master/include/float.h)          |
 | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
 | Source     | [{z88dk}/libsrc/math](https///github.com/z88dk/z88dk/tree/master/libsrc/math/)                                 |                                                                                         
 | Include    | #include `<math.h>`      |                                                                                                                                                                                 
 | Linking    | -lm    (or target specific alternative, i.e. -lmzx) |                                                                                                                                                    
 | Compile    |                        |                                                                                                                                                                                 
 | Comments   | none                                                                                                            |                                                                                        

The maths library 

========= Target specific alternatives =========
On some target other it is possible to optionally link-in smaller libraries interfacing to the native ROM code.   Such alternatives are normally slighlty weaker either in speed or precision but are still a good option when the program size becomes an issue.

The available options at the moment are:

 | Target platform ^ Small maths LIB ^ Even smaller option | 
 | ------------------------------------------------------- | 
 | Sinclair ZX 81 | -lm81 | -lm81_tiny |                  
 | Lambda 8300   | -lmlambda | -lmlambda_tiny |           
 | Sinclair ZX Spectrum | -lmzx | -lmzx_tiny |            
 | Cambridge Z88 | -lz88_math | |                         
 | Amstrad CPC | -lcpc_math | |                           
 | Amstrad CPC 464 | -l464_math | |                       
 | Amstrad CPC 664 | -l664_math | |                       
 | Amstrad CPC 6128 | -l6128_math | |                     


========= Floating Point Precision =========
The precision of an FP number is determined by the so called "EPSILON" value.    It is the smallest value which can be added to '1' still making a difference.

The given example tests the pre-defined FLT_EPSILON constant and tries to determine EPSILON experimentally to compare the empiric results to the compiler's presets.

Compiler command line examples:

    zcc +`<target>` -lndos -create-app -lm epsilon.c

    zcc +zx -lndos -create-app -lmzx -DFASTMATH epsilon.c


	
	
	#include `<stdio.h>`
	#include `<math.h>`
	#include `<float.h>`
	
	#ifdef DOUBLE
	double a,b;
	#else
	float a,b;
	#endif
	
	int exponent;
	
	
	double getexp(double value) {
		exponent=0;
		while ((int)value==0.0) {
			exponent--;
			value *= 10.0;
		}
		return(value);
	}
	
	
	
	int main() {
	  a=1;
	  while ((1.0+a)!=1.0)
	  {
		b=a;
		a=a/2.0;
	  }
	    
	  putchar(12);
	  getexp(b);
	  printf ("Computed value for Epsilon:   %fe%d\n", getexp(b),exponent);
	  if ((1.0+b)==1.0) {
		  printf ("..not confirmed.\n");
	  }
	
	  getexp(FLT_EPSILON);
	  printf ("Predefined value for Epsilon: %fe%d\n", getexp(FLT_EPSILON),exponent);
	
	  a=1.0;
	  b=a;
	  while ((a+FLT_EPSILON)>a) {
		b=a;
		a*=10.0;
	  }
	  a=b;
	  if (a>1.0) {
		  while (((a+FLT_EPSILON)>a)&&(b!=a)) {
			b=a;
			/* if the loop stales, try adding a minimum value, FLT_EPSILON is fine */
			/* a+=1.0+FLT_EPSILON; */
			a+=1.0;
		  }
		  a=b;
	  }
	  
	
	  if (a<10.0) {
		  printf ("FLT_EPSILON is very close to the real machine limit.\n");
	  } else {
		  if (a>2000.0)
			printf ("FLT_EPSILON is far from the real machine limit.\n");
	  }
	  printf ("\nEPSILON is relevant for values <= %f\n", a);
	
	  if ((1.0+FLT_EPSILON)==1.0) {
		  printf ("\nError: this value for FLT_EPSILON is not correct !\n\n");
	  }
	
	}
	

