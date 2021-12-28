# Installation

The [nightly build](http://nightly.z88dk.org/) is the most current version.  The package available for download from [sourceforge](https://sourceforge.net/projects/z88dk/) and Github is dated 7 Feb 2021.

The nightly build should be preferred unless you have a reason to install an older version of z88dk.  The documentation on this page will apply to the nightly build.

**NOTE: Some users have reported problems with usage because they have other unrelated programs installed named "zcc" or "z80asm" earlier in their paths.  If you are having build or compile trouble, try putting z88dk/bin at the front of your path to see if the problems go away.**

## Building from sources

To build z88dk from sources, the following tools are needed:
- gcc version 8 (supporting C++17 and std::filesystem)
- Ragel State Machine Compiler version 6.10
- re2c 1.3 - tool for generating fast C-based recognizers
- Perl 5

The tools required for the CI environment are listed in https://github.com/z88dk/z88dk/blob/master/.github/workflows/c-cpp.yml

## Docker usage

See [Docker Usage](Docker-Usage) for details - due to changes at dockerhub nightly builds are not available.

## Snapcraft usage

A snap file is available for certain Linux distributions. For more details see [Snap usage](Snap-usage).

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

This will create a populated z88dk directory in the current working directory. Alternatively you could clone the GitHub repository (including submodules) using:

    git clone  --recursive  https://github.com/z88dk/z88dk.git

You will need the following libraries/packages installed to successfully build z88dk:

```
build-essential
dos2unix
libboost-all-dev
texinfo
texi2html
libxml2-dev
subversion
bison
flex
zlib1g-dev
m4
```

Then enter:

    cd z88dk
    export BUILD_SDCC=1
    chmod 777 build.sh
    ./build.sh

This will build z88dk include `zsdcc`. If you don't want to build `zsdcc` then omit the `export BUILD_SDCC=1` line. The source code for sdcc is downloaded from svn which can be slow, if it keeps failing then run the command: `export BUILD_SDCC_HTTP=1` before prior to running `./build.sh` - this will download a source code tarball from the z88dk nightly website.

You can run z88dk keeping it in the current location, all you need to do is to set the following environment variable:

| Variable | Value               |
| -------- | -----               |
| `ZCCCFG` | `{z88dk}/lib/config`|

Where `{z88dk}` is the path to the z88dk directory.

Supposing you have `bash` (most likely it is your system default shell) and you want to keep z88dk in your local user environment (AKA 'home directory'), you can configure it permanently in this way:
    vi ~/.bash_profile

Modify the configuration as follows:

    export PATH=${PATH}:${HOME}/z88dk/bin
    export ZCCCFG=${HOME}/z88dk/lib/config

A system install is not fully supported at the moment, however if you wish to install z88dk and merge it with your default system environment, then edit 'z88dk/Makefile' and set your preferred destination position (default is /usr/local), then type: `make install` or alternatively on systems where /bin/sh is actually dash (eg Ubuntu) `make SHELL=/bin/bash install`. 

The build detailed above skips running many of the tests, to run them add the `-t` option to the build.sh invocation. In order to run the z80asm unit tests the following perl packages are required:

```
App::Prove Modern::Perl Capture::Tiny Capture::Tiny::Extended Path::Tiny File::Path Template Template::Plugin::YAML Test::Differences CPU::Z80::Assembler Test::HexDifferences Data::HexDump Object::Tiny::RW Regexp::Common List::Uniq
```

More details can found in `.travis.yml` which defines the required packages for the Travis CI build.


## MacOS X

The MacOS X build contains prebuilt binaries to simplify installation. Download the latest package and unzip to a directory:

    curl -O http://nightly.z88dk.org/z88dk-osx-latest.zip
    unzip z88dk-osx-latest.zip

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
	
	int main()
	{
	   printf("Hello World !\n");
           return 0;
	}


Open a terminal or command prompt, change to the directory where the test program was saved and compile the program with:

	
	zcc +zx -vn test.c -o test -lndos


There should be no errors.

## (Optional) installation of clang+llvm

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
	
	zcc +z80 -vn -SO3 -clib=clang_iy --max-allocs-per-node200000 test.c -o test -create-app
