# Installation

The [nightly build](http://nightly.z88dk.org/) is the most current version.  The package available for download from [sourceforge](https://sourceforge.net/projects/z88dk/) is dated 10 Jan 2017.

The nightly build should be preferred unless you have a reason to install an older version of z88dk.  The documentation on this page will apply to the nightly build.

**NOTE: Some users have reported problems with usage because they have other unrelated programs installed named "zcc" or "z80asm" earlier in their paths.  If you are having build or compile trouble, try putting z88dk/bin at the front of your path to see if the problems go away.**

## Windows

Download the latest nightly windows build and unzip it into the destination directory (we suggest to avoid spaces in the directory names).  This will create a tree rooted in a z88dk subdirectory.

Some environment variables will have to be defined.  On Windows 8, this can be done from the Control Panel by selecting "User Accounts".  On the left side of the pop-up box you should find a link to "Change my environment variables".  Click that and add the following:


| Variable | Value              |
| -------- | -----              |
| ZCCCFG   | {z88dk}\lib\config |


Finally, add ";{z88dk}\bin" to Path.

In the above "{z88dk}" is the full path to the z88dk directory created.

To update the install simply delete the old one, download a new nightly build and unzip in the same location.

Any command prompt that is opened will be ready to compile using z88dk.

## Linux / Unix

Download the latest nightly checked source package and unzip it:

    wget http://nightly.z88dk.org/z88dk-latest.tgz
    tar -xzf z88dk-latest.tgz

This will create a populated z88dk directory in the current working directory.

To succeed in building the 'z80svg' graphics tool you need the 'libxml2' library to be previously installed, although its absence will not prevent the rest of the kit from building.

Then enter:

    cd z88dk
    chmod 777 build.sh
    chmod 777 config.sh
    ./build.sh

You can run z88dk keeping it in the current location, all you need to do is to set the following environment variable:

| Variable | Value              |
| -------- | -----              |
| ZCCCFG   | {z88dk}/lib/config |

Supposing you have `bash` (most likely it is your system default shell) and you want to keep z88dk in your local user environment (AKA 'home directory'), you can configure it permanently in this way:
    vi ~/.bash_profile

Modify the configuration as follows:

    export PATH=${PATH}:${HOME}/z88dk/bin
    export ZCCCFG=${HOME}/z88dk/lib/config

A system install is not supported in this release.

To complete installation you may want to build sdcc, details below.

Otherwise, if you wish to install z88dk and merge it with your default system environment, then edit 'z88dk/Makefile' and set your preferred destination position (default is /usr/local), then type:  **make install** (you still should add the two environment variables in the system settings).

## MacOS X

The MacOS X build contains prebuilt binaries to simplify installation. Download the latest package and unzip to a directory:T

    wget http://nightly.z88dk.org/z88dk-osx-latest.tgz
    tar -xzf z88dk-osx-latest.tgz

You can run z88dk keeping it in the current position, all you need to do is to set the following environment variable:

| Variable | Value              |
| -------- | -----              |
| ZCCCFG   | {z88dk}/lib/config |

Supposing you have **bash** (most probably it is your system default shell) and you want to keep z88dk in your local user environment (AKA 'home directory'), you can configure it permanently in this way:
    vi ~/.bash_profile

Modify the configuration as follows:

    export PATH=${PATH}:${HOME}/z88dk/bin
    export ZCCCFG=${HOME}/z88dk/lib/config

To update the install simply delete the old one, download a new nightly build and unzip in the same location.


## Verify the Install

To test if the install was successful, create the following program in a text editor and save as "test.c"

	#include <stdio.h>
	
	main()
	{
	   printf("Hello World !\n");
	}


Open a terminal or command prompt, change to the directory where the test program was saved and compile the program with:

	
	zcc +zx -vn test.c -o test -lndos


There should be no errors.

# Installation of Support Tools (Optional)

These tools are supplied by third parties.

## m4

[M4 Manual (v1.4.14)](https://www.gnu.org/software/m4/manual/m4-1.4.14/m4.html) /
[Notes on the M4 Macro Language](http://mbreen.com/m4.html)

m4 is the standard macro processor used on Linux and Unix machines.  It has now been adopted as an optional macro pre-processor in z88dk.  The nightly build and latest windows package downloadable at sourceforge now include the m4 binary for windows machines so that m4 is now available to all installs.

zcc will pre-process source files ending in .m4 extension using m4.  The intention is to supply a macro pre-processing facility for files ending in .c.m4 / .asm.m4 / .inc.m4 / .h.m4 extensions.

zcc will apply m4 and then immediately write a macro expanded file to its original source directory as .c / .asm / .inc or .h so that it is available in the current compile.  .c and .asm expanded files are then processed as normal by zcc.

**Note that files in the form *.ext.m4 will result in a new file *.ext generated in the same directory, overwriting any file of the same name there.  Source files of the form *.ext.m4 should be understood to also reserve the name *.ext in the same directory.**

m4 is also used by the new c library to generate its crts from macros.


## sdcc

[SDCC Main Page @ Sourceforge](https://sourceforge.net/projects/sdcc/) /
[SDCC Nightly Build Page](http://sdcc.sourceforge.net/snap.php) /
[SDCC Manual](http://sdcc.sourceforge.net/doc/sdccman.pdf)

sdcc is an open source optimizing C compiler that can target the z80.  A patched version is compatible with z88dk and can be invoked by zcc when the appropriate flag is selected on the command line.  Besides making sdcc compatible with the z88dk toolchain, the patch also improves sdcc's generated code, addresses some of sdcc's code generation bugs and, being part of z88dk, grants sdcc access to z88dk's assembly language libraries and ready-made crts.

** 1. Windows **

The [z88dk nightly build](http://nightly.z88dk.org/) for windows is now self-contained and includes the zsdcc binary in z88dk/bin.  Separate installation of sdcc is no longer necessary.  [sdcc_z88dk_patch.zip](https://github.com/z88dk/z88dk/blob/master/libsrc/_DEVELOPMENT/sdcc_z88dk_patch.zip) will often contain a more recent windows build of zsdcc that can be copied into z88dk/bin.

** 2. Mac OSX **

The [z88dk nightly build](http://nightly.z88dk.org/) for mac osx is now self-contained and includes the zsdcc binary in z88dk/bin.  Separate installation of sdcc is no longer necessary.

** 3. Linux / Unix **

Other users will have to apply the svn patch found in [sdcc_z88dk_patch.zip](https://github.com/z88dk/z88dk/blob/master/libsrc/_DEVELOPMENT/sdcc_z88dk_patch.zip) and build sdcc from source.

A typical linux install process for sdcc would look like this:


*  **`svn checkout svn://svn.code.sf.net/p/sdcc/code/trunk sdcc-code`**  This will check out the current development version of sdcc.  If you already have the sdcc-code tree from a previous checkout you can instead perform an update.

*  **copy "sdcc-z88dk.patch" from inside [sdcc_z88dk_patch.zip](https://github.com/z88dk/z88dk/blob/master/libsrc/_DEVELOPMENT/sdcc_z88dk_patch.zip) into the sdcc-code directory**

*  **`cd sdcc-code`**

*  **`patch -p0 < sdcc-z88dk.patch`**  Apply the z88dk patch.

*  **`cd sdcc`**

*  **`./configure`**  Some additional packages may have to be installed to complete configuration.  On Knoppix 7.6.0 these were flex, bison, gputils, libboost-dev.

*  **`make`**  The build will fail when the non-z80 device libraries are compiled.  This is expected and is the reason the z88dk patch has not been accepted into sdcc; the resulting binary is error-free for z80 compilation only.

*  **`cd bin`**

*  **`mv sdcc {z88dk}/bin/zsdcc`**  Move the patched sdcc executable to {z88dk}/bin and rename it "zsdcc".

*  **`cp sdcpp {z88dk}/bin/zsdcpp`** Copy the sdcc preprocessor to {z88dk}/bin and rename it "zsdcpp".

*  **`cd ../..`**  Back to sdcc-code.

*  **`patch -Rp0 < sdcc-z88dk.patch`**  Undo the patch.  We will re-build sdcc from original source so that sdcc is available in its original form.

If you have an existing sdcc install or you don't want to continue with an install of sdcc you can stop here and verify the install was successful below.  Optionally keeping the sdcc source tree in an unpatched state can allow you to update the zsdcc binary by repeating the steps above as sdcc itself is updated.  Both z88dk and sdcc are active projects that see frequent updates.

To complete sdcc installation continue with these steps:


*  **`cd sdcc`**

*  **`make`**  The build process should now complete.

*  **`sudo make install`**

** VERIFY THE INSTALL **

"zsdcc" is the name of the patched sdcc executable and "zsdcpp" is the name of the sdcc C preprocessor that z88dk will invoke.

Entering "zsdcc -v" and "zsdcpp --version" should print version information.  The version information for zsdcc should begin with "ZSDCC is a modification of SDCC for Z88DK".  If that is not the case, the system is executing an older version of zsdcc from the sdcc/bin directory rather than the new version in z88dk/bin.  The older version would have been installed by following an older version of these instructions.  Find the zsdcc executable in sdcc/bin and remove it.

To verify that sdcc is usable from z88dk, try compiling [sudoku.c](https://raw.githubusercontent.com/z88dk/z88dk/master/libsrc/_DEVELOPMENT/EXAMPLES/sudoku.c) for the cp/m target using sdcc:

	
	zcc +cpm -vn -SO3 -clib=sdcc_iy --max-allocs-per-node200000 sudoku.c -o sudoku -create-app


## clang+llvm

(recent and not quite ready)

[LLVM + SDCC Toolchain](http://www.colecovision.eu/llvm+sdcc/) \\ 
[Clang 3.8 Manual](http://llvm.org/releases/3.8.1/tools/docs/UsersManual.html) \\ 
[LLVM 3.8 Documentation](http://llvm.org/releases/3.8.0/docs/)

In order to compile using clang, you must also have [sdcc installed](temp/front#sdcc1).

Clang is a C front end that translates C to LLVM intermediate form and LLVM functions as the back end performing various optimizations on the way to generating the output.  Clang+LLVM are well known projects in the open source community, probably made most famous by Apple which uses it as its C compiler.

LLVM as-is is not well suited for targeting small microprocessors.  It would take a considerable amount of work to persuade it to generate as good code as the currently available non-LLVM z80 compilers.  However, there is a fruitful shortcut that can be taken to exploit some of LLVM's strengths and that is using Clang to compile C to LLVM intermediate form and then using LLVM to perform optimizations and output as C.  Then that C can be compiled by an existing z80 C compiler to a binary.  In this case, sdcc will be used as the z80 C compiler because it best supports the modern C code that LLVM will be producing.

This process is not as wasteful as it sounds -- there is some indication that a Clang/LLVM/SDCC sequence might produce better code.  However, the real prize is that the LLVM toolchain can be used to compile other languages to C and this is an avenue z88dk would like to explore in the future.

** 1. Windows **

** 2. Mac OSX **

** 3. Linux / Unix **

(installation instructions coming; linux/unix users can perhaps follow the instructions in the toolchain link above but rename the binaries to zclang and zllvm-cbe; compiles use the new c library with sdcc command line switches and "-clib=clang_iy" or "-clib=clang_ix")

	
	zcc +embedded -vn -SO3 -clib=clang_iy --max-allocs-per-node200000 test.c -o test -create-app

## Boost Libraries

### On Windows

Follow [Install Boost](https://docs.microsoft.com/en-us/visualstudio/test/how-to-use-boost-test-for-cpp?view=vs-2017#install-boost)

or, condensed version:

	git clone https://github.com/Microsoft/vcpkg
	cd vcpkg
	bootstrap-vcpkg.bat   (Windows)
	./bootstrap-vcpkg.sh  (Linux, MacOS)
	vcpkg install boost

- Set environment variable BOOST = ...path-to-git-clone/vcpkg/installed/x86-windows/include; 
- use -I$(BOOST) in Makefiles
- use $(BOOST) in Visual Studio project properties->General->Additional Include Directories

### On Ubuntu/Debian

	sudo apt-get install libboost-all-dev
