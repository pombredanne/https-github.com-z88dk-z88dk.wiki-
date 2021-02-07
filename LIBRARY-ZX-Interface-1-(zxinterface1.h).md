### Interface 1 (`<zxinterface1.h`> `-lzxmdv`)

Low level access to microdrives is available by linking to the `zxmdv` library. The library requires a working `malloc()` heap.

### Data Structures

All the Interface 1 data structures used for operations on "streams" begin in the same way.

The common heading is the defined separately:


```c
    struct BASE_CHAN {
            u16_t   errptr1;        /* first pointer to main ERROR-1     */
            u16_t   errptr2;        /* second pointer to main ERROR-1    */
            u8_t    id_char;        /* inverted or regular "M"/"N" char  */
            u16_t   out;            /* pointer to the output routine     */
            u16_t   in;             /* pointer to the input routine      */
            u16_t   len;            /* length of channel                 */
    }; 
```
### ZX Microdrive

This group of functions mainly work by calling directly the low level portion of the ROM routines (which are automatically localized for the three existing firmware versions) to overcome some limitations and bugs.
The sector-seek loop has been totally recreated to avoid the BASIC interpreter imposing itself when an I/O error occurs.

The buffers aren't stored before the BASIC program area anymore (they are part of the C data structure) and the number of files being opened at once is limited by the tape and memory space only.

The sectors can be identified both as a unique block in the whole tape and as a specific record in a file.  They can be loaded, created or modified an then saved on the same or on a different sector.

The core element to access to a sector is the "channel definition", which include the stream properties and the sector buffer and some trailing data permitting advanced file operations (lseek, etc):



```c
    // M_CHAN is 603 bytes long
    struct M_CHAN {

            // base channel descriptor
            struct  BASE_CHAN base;

            // "M" channel specific stuff
            u16_t   bytecount;      /* (IX+$0B) Count bytes in record */
            u8_t    record;
            char    name[10];       /* file name */
            u8_t    flag;
            u8_t    drive;          /* drive number (0-7)*/
            u16_t   map;            /* Address of MAP for this microdrive.*/
            char    hdpreamble[12]; /* 12 bytes of header preamble */
            u8_t    hdflag;
            u8_t    sector;         /* sector number */
            u16_t   unused;
            char    hdname[10];     /* cartridge name */
            u8_t    hdchk;          /* Header checksum */
            char    dpreamble[12];  /* 12 bytes of data block preamble */
            u8_t    recflg;         /* bit 1 set for EOF, bit 2 set for PRINT file type */
            u8_t    recnum;         /* Record number in the range 0-255 */
            u16_t   reclen;         /* (IX+$45) Number of databytes in record 0-512 */
            char    recname[10];    /* file name */
            u8_t    recchk;         /* Record  description checksum */
            char    data[512];      /* the 512 bytes of data. */
            //char    datahd[10];   /* first 9 bytes of the 512 bytes of data. */
            //char  data[502]       /* real program */
            u8_t    datachk;        /* Checksum of preceding 512 bytes */

            /* These values are added for the file handling
               the ROM shouldn't overwrite those fileds */
            long    position;       /** NEW** - current position in file */
            int     flags;
            mode_t  mode;
        };
```
#### Functions - Diagnostics

`int if1_from_mdv()`

Returns true if the current program has been loaded from microdrive

`int if1_installed()`

Returns true if the system variables are already present

`int zx_interface1()`

Returns true if the Interface 1 is present

#### Functions - Microdrives

`int if1_load_record (int drive, char *filename, int record, struct M_CHAN *buffer)`

Load a sector identified by file name and record number, returning the current sector number, or '-1' if any error.

*An M_CHAN buffer structure pointing to a valid memory space must be passed.*

`int if1_load_sector (int drive, int sector, struct M_CHAN *buffer)`

Load a sector identified by the specified sector number returning the sector number being loaded, or '-1' if any error.

*An M_CHAN buffer structure pointing to a valid memory space must be passed.*

`int if1_write_sector (int drive, int sector, struct M_CHAN *buffer)`

Write the sector present in "buffer", returning '0' if everything goes fine, '-1' on error.

*An M_CHAN buffer structure pointing to a valid memory space and initialized with the proper values must be passed.*

`int if1_write_record (int drive, struct M_CHAN *buffer)`

Add a record containing additional data in "buffer", returning '0' if everything goes fine, '-1' on error.

*An M_CHAN buffer structure pointing to a valid memory space and initialized with the proper values must be passed.*

`int if1_setname(char* name, char *location)`

Put a 10 characters file name at the specified location; return with the file name length

`char *if1_getname(char *location)`

Pick the file name at the given location and convert it into the C standard string format.

`int if1_remove_file(int drive, char *filename)`

Delete a file

`int if1_touch_file(int drive, char *filename)`

Create a file if it doesn't exist

`int if1_init_file (int drive, char *filename, struct *M_CHAN buffer)`

Create a file and return a file handle.

*An M_CHAN buffer structure pointing to a valid memory space must be passed.*

`void if1_update_map (int drive, char *mdvmap)`

Load the map values for the specified drive

`int if1_find_sector (int drive)`

Find a free sector

`int if1_find_sector_map (char *mdvmap)`

Find a free sector in the specified map
