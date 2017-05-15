# Creating Z88 Packages

### Introduction

Z88 packages, are analogous to shared libraries, the infrastructure is provided by the Installer application within the [Z88 Forever](http://www.worldofspectrum.org/z88forever/rom-forever.html) application. Documentation for packages can be found on this page [http://www.worldofspectrum.org/z88forever/technical.htm](http://www.worldofspectrum.org/z88forever/technical.htm).

A package is always attached to a application eg. The Packege Package is
attached to Installer and the TCP Package is attached to the application
ZSock, thus building a package with z88dk is tied to building an
application. As result this documentation is written with the assumption
that you know how to write and build applications with z88dk.

### Constructing The Package Dor

The package DOR is the structure which informs the Package system about
your package. Creating the DOR with z88dk is incredibly simple. Normally
to create an application DOR you would do the following:

	#include `<dor.h>`
	
	/* Define application paramaters */
	
	#include `<application.h>`


In a similar vein, to build the package DOR you do the following:

	#include `<dor.h>`
	
	/* Define package parameters */
	
	#include `<package.h>`
	/* Define table of pointers to functions (jump table)*/
	
	/* Define application parameters */
	
	#include `<application.h>`


The package parameters are as follows:

	#define MAKE_PACKAGE 1


Say that you do want to create a package(!

	#define PACK_VERSION    $MMmm


The version of the package you are creating - MM=major, mm=minor

	#define PACK_NAME       "[name]"


The name of the package (without the [] of course!)

	#define PACK_BOOT


This should be set to either 0 or 1 depending on whether the package
should autoboot or not (This is of course bypassed by holding down 'P'
on reset)

	#define PACKAGE_ID      $xx


The package number as allocated by Garry Lancaster

	#define MAX_CALL_NUM $nn


The maximum LSB of a call to your package.

### Setting Up The Jump Table

The jump table can be easily defined using the following construct:

	package_str jumptable[] = {
	        {Func1},
	        {Func2},
	        ....
	        {FuncN}
	};


Remember to prototype as external to the file Func1....FuncN

### Finally

You have to supply 3 functions yourself to fully define the package, these are the pack_ayt, pack_bye and pack_dat functions.  Due to the complex entry and exit parameters of these functions they are best written in assembler. For more details about these calls please see Garry's documentation. These functions must either be prototyped (to allow correct linking)



