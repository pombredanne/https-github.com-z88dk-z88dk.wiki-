# RS232 Serial Interfaces API (rs232.h)

 | Header     | [{z88dk}/include/rs232.h](https///raw.githubusercontent.com/z88dk/z88dk/master/include/rs232.h?view=markup)    |
 | -----------------------------------------------------------------------------------------------------------------------------
 | Source     | [{z88dk}/z88dk/libsrc/rs232](https///github.com/z88dk/z88dk/tree/master/libsrc/rs232)                     |     
 | Include    | #include `<rs232.h>`           |                                                                                  
 | Linking    | n/a                          |                                                                                  
 | Compile    | n/a                          |                                                                                  
 | Comments   |                              |                                                                                  

Support for RS232.  Not all the platforms are supported, at the moment.


	/* Handshaking codes used in XMODEM and its derivatives */

	#define	SOH    1
	#define	EOT    4
	#define	ACK    6
	#define	DLE    16
	#define	DC1    17    /* X-On  */
	#define	DC3    19    /* X-Off */
	#define	NAK    21
	#define	SYN    22
	#define	CAN    24


# RS232 API

	/* Error codes returned by all functions */
	#define RS_ERR_OK                       0x00    /* Not an error - relax */
	#define RS_ERR_NOT_INITIALIZED          0x01    /* Module not initialized */
	#define RS_ERR_BAUD_TOO_FAST            0x02    /* Cannot handle baud rate */
	#define RS_ERR_BAUD_NOT_AVAIL           0x03    /* Baud rate not available */
	#define RS_ERR_NO_DATA                  0x04    /* Nothing to read */
	#define RS_ERR_OVERFLOW                 0x05    /* No room in send buffer */


## rs232_params(params, parity)

Set up the parameters for the serial interface use | of parameters above

	/* Baudrate settings */
	#define RS_BAUD_50                      0x00
	#define RS_BAUD_75                      0x01
	#define RS_BAUD_110                     0x02
	#define RS_BAUD_134_5                   0x03
	#define RS_BAUD_150                     0x04
	#define RS_BAUD_300                     0x05
	#define RS_BAUD_600                     0x06
	#define RS_BAUD_1200                    0x07
	#define RS_BAUD_2400                    0x08
	#define RS_BAUD_4800                    0x09
	#define RS_BAUD_9600                    0x0A
	#define RS_BAUD_19200                   0x0B
	#define RS_BAUD_38400                   0x0C
	#define RS_BAUD_57600                   0x0D
	#define RS_BAUD_115200                  0x0E
	#define RS_BAUD_230400                  0x0F

	/* Stop bit settings */
	#define RS_STOP_1                       0x00
	#define RS_STOP_2                       0x80

	/* Data bit settings */
	#define RS_BITS_5                       0x60
	#define RS_BITS_6                       0x40
	#define RS_BITS_7                       0x20
	#define RS_BITS_8                       0x00

	/* Parity settings */
	#define RS_PAR_NONE                     0x00
	#define RS_PAR_ODD                      0x20
	#define RS_PAR_EVEN                     0x60
	#define RS_PAR_MARK                     0xA0
	#define RS_PAR_SPACE                    0xE0



## rs232_init()

Initialise the serial interface 

## rs232_close()

Close the interface 

## rs232_put(char)

Write a byte to the serial interface 

## rs232_get(char *)


Read a byte from the serial, returns RS_ERR_NO_DATA if an error, places data in the pointer supplied

# API test program

The following program requires a serial terminal connected to the serial port to test.


	
	
	#include `<stdio.h>`
	#include `<stdlib.h>`
	#include `<rs232.h>`
	
	char byte[1];
	int f;
	
	int main()
	{
		printf ("%cChecking basic RS232 capabilities...\n",12);
		if (rs232_params(RS_BAUD_1200, RS_PAR_NONE) == RS_ERR_OK)
			printf ("  1200 baud, N, 8, 1\n");
		
		if (rs232_params(RS_BAUD_2400, RS_PAR_NONE) == RS_ERR_OK)
			printf ("  2400 baud, N, 8, 1\n");
	
		if (rs232_params(RS_BAUD_4800, RS_PAR_NONE) == RS_ERR_OK)
			printf ("  4800 baud, N, 8, 1\n");
	
		if (rs232_params(RS_BAUD_9600, RS_PAR_NONE) == RS_ERR_OK)
			printf ("  9600 baud, N, 8, 1\n");
	
		if (rs232_params(RS_BAUD_19200, RS_PAR_NONE) == RS_ERR_OK)
			printf ("  19200 baud, N, 8, 1\n");
			
		printf ("\nInitializing at 1200 baud:");
		if (rs232_params(RS_BAUD_1200, RS_PAR_NONE) != RS_ERR_OK) {
			printf ("  Error setting baud rate.  Exiting...\n");
			exit(0);
		}
		if (rs232_init() != RS_ERR_OK) {
			printf ("  Initialization error.  Exiting...\n");
			exit(0);
		}
		printf ("  Done.\n");
	
	
		rs232_put('-');
		rs232_put('>');
		
		printf ("\nEchoing 10 bytes to console: ");
		for (f=0; f<10; f++) {
			while (rs232_get(&byte[0]) != RS_ERR_OK);
			//printf ("%c",byte[0]);
			fputc_cons (byte[0]);
		}
	
		rs232_put('.');
	
		printf ("\n\nClosing RS232 port:\n");
		if (rs232_close() != RS_ERR_OK) {
			printf ("  Error.  Exiting...\n");
			exit(0);
		}
	}
	


# Available RS232 libraries

The current versions are at the moment avaiable

## ZX Spectrum

(The [Paul Farrow's website](http://www.fruitcake.plus.com/Sinclair/Interface2/Cartridges/Interface2_RC_New_RS232.htm) has a nice section about the RS232 cabling)

    * **rs232if1.lib**, ZX Interface 1
    * **rs232plus.lib**, ZX Spectrum 128, and Plus/clones
    

## Amstrad CPC


    * **rs232cpc_booster.lib**, 'booster' rs232 interface
    * **rs232cpc_sti.lib**, STI interface
    

