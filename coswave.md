# 3D function drawing

This program will display the cos(x²+y²) function using the z-buffer algorithm.

```

#include `<graphics.h>`

#include `<stdio.h>`
#include `<math.h>`

main()
{
float x,y;
char z,buf;
	clg();

	for (x=-3.0; x<3.0; x=x+0.06)
	{
		buf=100;
		for (y=-3.0; y<3.0; y=y+0.2)
		{
			z = (char) 70.0 - (10.0 * (y + 3.0) + ( 10.0 * cos (x*x + y*y) ));
			if (buf>z)
			{
				buf = z;
				plot ( (char) (16.0 * (x+3.0)), (char) z);
			}
		}
	}
}

```



## Running on the Amstrad CPC


#### Basic environment

Compile it with the following command:

    zcc +cpc -lndos -lm -ocoswave -create-app coswave.c

It will make a file named "coswave.cpc", producing the following output:


{{examples:snippets:cpc_base.gif|}}


#### CP/M 2.X and CP/M Plus

Compile it with the following command:

    zcc +cpm -lndos -lcpccpm -lm coswave.c

It will create the "a.com" command:

{{examples:snippets:cpc_cpm.gif|}}

{{examples:snippets:cpc_cpmplus.gif|}}

#### Lambda 8300

Compile it with the following command:

    zcc +lambda -create-app -lm coswave.c

It will create the "a.p" program:

{{:examples:snippets:coswave-lambda.png|}}

