	/*
		Disciple Disk backup tool
		By Stefano Bodrato, (c) 2014
		
		zcc +zx -lndos -create-app -o discdump discdump.c

	*/

	#include <stdio.h>
	#include <stdlib.h>
	#include <spectrum.h>
	//#include <zxinterface1.h>

	int t,s;
	int k;

	void rdsec(int trk, int sec)
	{
	#asm
		pop bc
		pop hl
		pop de
		ld  d,l
		ld  a,1
		ld  ix,60000
		push bc
		rst 8
		defb 68
		ret
	#endasm
	}

	void wrsec(int trk, int sec)
	{
	#asm
		pop bc
		pop hl
		pop de
		ld  d,l
		ld  a,1
		ld  ix,60000
		push bc
		rst 8
		defb 69
		ret
	#endasm
	}


	int main()
	{
		printf ("%cPress 'b' to backup\n",12);
		printf ("Press 'r' to restore\n");
		k=getk();
		while ((k!='b')&&(k!='r')) k=getk();
		if (k=='b') {
			for (t=0;t<=79;t++)
				for (s=1;s<=10;s++) {
					rdsec(t,s);
					fputc_cons('.');
					tape_save_block(60000, 512, 2);
					rdsec(t+128,s);
					fputc_cons('.');
					tape_save_block(60000, 512, 2);
				}
		} else {
			printf ("Ready to overwrite disk ?\n");
			while (getk()!='y') {}
			for (t=0;t<=79;t++)
				for (s=1;s<=10;s++) {
					tape_load_block(60000, 512, 2);
					fputc_cons('.');
					wrsec(t,s);
					tape_load_block(60000, 512, 2);
					fputc_cons('.');
					wrsec(t+128,s);
				}
		}

		printf ("\n\nDisk backup complete !\n");

	}


