# INSTALLATION

You can choose to install the latest stable release or the current version from CVS.  This wiki documents the z80 libraries as they are currently implemented in CVS.

The latest stable release will be missing several of the libraries documented here.  However the libraries that are included are less likely to contain bugs and are less likely to have their APIs changed.  The new libraries being developed in CVS may contain bugs and may have their APIs change to improve their usefulness.  Since the time between stable releases tends to be long, it is recommended that you install the CVS version if the latest stable release is beginning to look a little bit old.

# Latest Stable Release

The latest stable release can be had from z88dk's [sourceforge download page](http://sourceforge.net/project/showfiles.php?group_id=2917).  Scroll down to file releases and select the appropriate package to download for your platform.  For Win32 and MacOSX there are binary packages available.  Other platforms should get the generic C source.  In the case of the latter, the z88dk binaries will have to be compiled with the help of a native C compiler and the guidelines mentioned below.

## MacOS X

##  Windows 95, 98 

**Unfortunately the exe installer only runs on Windows ME and above.  For earlier versions of Windows follow these steps:**


*  Download and unzip the [generic 1.8 zip file](http://downloads.sourceforge.net/z88dk/z88dk-src-1.8.tgz) into c:\.  A c:\z88dk directory will be created while unzipping.

*  Download the [win32 binaries](http://downloads.sourceforge.net/z88dk/z88dk-win32-1.8.zip) and unzip that into a temporary directory.  Inside the temporary directory a new directory z88dk.bin.win32 will be created.

*  Drag and drop all the files contained in the z88dk.bin.win32 directory into c:\z88dk.  Answer yes to all to overwrite any existing files.

*  Within c:\z88dk there should now be a bin directory; if not you have dragged and dropped incorrectly and you should repeat the last step.

*  It is now safe to delete the temporary directory.

**The following steps install native Win32 [Gnu Utils](http://sourceforge.net/projects/unxutils/) and are necessary if you would like to compile the z80 libraries yourself.  Installer comes with precompiled z80 libraries so this is optional.**


*  Download and unzip the [gnu utils](http://sourceforge.net/projects/unxutils/) into c:\gnu.

**The installation is complete with the exception of setting up environment variables which is described below.**

__NOTE__: Without the help of an EXE installer it requires considerable effort to perform an install into any directory other than c:\z88dk.

## Windows ME, 2000, XP

**Follow these steps for installation:**


*  Download and run the [win32 exe installer](http://downloads.sourceforge.net/z88dk/z88dk-1.8.0-setup.exe).  The installer allows z88dk to be placed in any directory of your choice, though c:\z88dk is recommended.

*  Unzip the zip files contained in {z88dk}\libsrc, {z88dk}\src and {z88dk}\support in place.

**The following steps install native Win32 [Gnu Utils](http://sourceforge.net/projects/unxutils/) and are necessary if you would like to compile the z80 libraries yourself.  The install comes with precompiled z80 libraries so this is optional.**


*  Download and unzip the [gnu utils](http://sourceforge.net/projects/unxutils/) into c:\gnu.

**The installation is complete with the exception of setting up environment variables which is described below.**

## CygWin

## MinGW32 + MSYS

# Nightly built packages

The current CVS version is re-built every night to check its consistency and simplify its installation for people wishing to experiment the latest features with little or no hassle.
When the code is consistent enough a new binary archive is created:

[z88dk nightly build repository](http://nightly.z88dk.org/)

Its use is now quite straightforward: just extract the files, extend the path to include the '{z88dk}/bin' folder and zcc will be ready for you.
By putting the files in a default position (i.e. c:\z88dk for Windows) no extra environment variables configuration is required.


# Current CVS Version

Z88dk's [CVS source repository](http://sourceforge.net/cvs/?group_id=2917) is maintained at Sourceforge.  To grab the latest CVS version you will need a CVS client running on your platform that will contact Sourceforge and either retrieve the repository or update the local version of the repository that you already have.  The link just mentioned and this [page](http://sourceforge.net/docs/E04/) provide instructions on how to set up CVS on your platform.  Further instructions for specific architectures can be found below.

## Windows 95, 98

## Windows ME, 2000, XP

**[Tortoise CVS](http://tortoisecvs.sourceforge.net/) is highly recommended as CVS client software for Win32.  It integrates directly into Windows Explorer allowing CVS actions to be initiated from the context menu associated with files and folders.  To install Tortoise CVS follow these steps:**


*  Download and run the latest [Tortoise CVS exe installer](http://prdownloads.sourceforge.net/tortoisecvs/TortoiseCVS-1.8.29.exe).

*  Restart your computer.

**We will use the 1.8 installer to generate files containing correct path information for the destination directory chosen for z88dk.  Toward that end, follow these steps:**


*  Download and run the [win32 exe installer](http://downloads.sourceforge.net/z88dk/z88dk-1.8.0-setup.exe).  The installer allows z88dk to be placed in any directory of your choice, though c:\z88dk is recommended.

*  With the install complete, drag and drop the {z88dk}\bin and {z88dk}\lib\config directories to the desktop.  These directories contain the win32 binaries and the files the installer generated with correct path information.

*  Uninstall the install just performed by running {z88dk}\unins000.exe

**The following steps use Tortoise CVS to obtain a local copy of z88dk's CVS tree:**


*  Navigate to the directory where you would like to create the z88dk tree with windows explorer.  This should be the same directory where the exe installer installed above.

*  Right click in the folder to bring up the context menu and select "CVS checkout".  If you do not see this option, Tortoise CVS has not been installed correctly or you have not restarted your computer.

*  Fill in the dialog that pops up as follows:
    * Protocol: pserver
    * Server: z88dk.cvs.sourceforge.net 
    * Repository Folder: /cvsroot/z88dk
    * Username: anonymous
    * Module: z88dk

*  Press OK

**Tortoise CVS will contact Sourceforge and make a local copy of the z88dk repository.  After a few minutes you will have a new z88dk directory with everything in it. In explorer, CVS directories will have a green checkmark next to them to indicate they are up-to-date.  Files that you have modified will have an orange arrow to remind you they are different from what is in the repository.  Files with blue question marks are your own files that do not come from the z88dk repository.**

**With the local copy of the repository present, replace the z88dk win32 binaries and config files dragged to the desktop earlier:**


*  Drag the bin directory from the desktop to {z88dk}

*  Drag the config directory from the desktop to {z88dk}\lib.  Answer yes to all to allow overwriting of files.

**The following steps install native Win32 [Gnu Utils](http://sourceforge.net/projects/unxutils/) and are necessary in order to compile the z80 libraries yourself.  The z80 libraries retrieved via CVS are not necessarily the latest compiles.  It is recommended that you [make the z80 libraries](unknown) yourself following the install.  This is described in the next section of the wiki.**


*  Download and unzip the [gnu utils](http://sourceforge.net/projects/unxutils/) into c:\gnu.

**The CVS install is complete with the exception of setting up environment variables which is described below.**

__NOTE__:  If any bugfixes are announced or you would like to stay current with the latest in z88dk, simply right click on the z88dk directory and choose "update" to have Tortoise CVS get updates from the repository.  It is that easy to stay up-to-date.

## Other


# Environment Variables

## PATH

Very little to say, just note that as for almost every programming kit z88dk requires the PATH environment variable to include the `<application root>`/z88dk/bin** directory.

## ZCCCFG

It is used by ZCC to find the target dependent configuration files.   It should be set to point to `<application root>`/z88dk/lib/config/**.
The trailing 'slash' (or backslash) character matters.

## Z80_OZFILES

It is the root path for the library files.  It should be set to point to `<application root>`/z88dk/lib/**.




# Navigating z88dk's Directory Tree

The current z88dk tree can be navigated online, on the [git repository](https://github.com/z88dk/z88dk/).

Here is a short description on how the sources are divided:


*  **bin/**  - target place for the z88dk compiler and tools

*  **doc/**  - mostly outdated documentation, the latest docs are in this wiki site

*  **examples/** - few examples, some are outdated; find more on this wiki site

*  **include/**  - C definitions for the z80 libraries

*  **lib/**      - core of the z88dk compiler environment
    * **clibs/**   - compiled z80 libraries
    * **config/**  - zcc configurations files for specific targets

*  **libsrc/**   - z80 libraries sources

*  **src/**      - compiler and tools sources

*  **support/**  - various support tools for several target environments

*  **test/**     - z80 libraries test suite

# Compiling the z88dk Binaries

