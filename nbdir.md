# Seeking the disk directory in an expanded NewBrain

```
/*

 *	Example of advanced use of the NewBrain specific functions
 *	Display disk directory in expanded NewBrain
 */

#include `<stdio.h>`

#include `<newbrain.h>`
#include `<string.h>`

#define NBCHAN 100

main()
{
char buf[150];

	/*  Open the directory device on stream 15 */
	nb_open( OUTP, NBCHAN, DEV_SDISCIO, 0, "" );
	
	/* send 'DIR' command (3) for 'A:*.*' to directory device */
	nb_puts( NBCHAN, "3A:*.*\n" );
	
	while (strlen( nb_gets(NBCHAN, buf) )>0) {
	  printf ("%s\n",buf);
	  nb_puts( NBCHAN, "4");	/* ask for next item */
	}

	nb_close( NBCHAN );
}

```
