# (Re)building the z88dk libraries

The z88dk libraries are placed in the {z88dk}/libsrc folder.
The compiler itself must be built (or installed) already and the system environment must be setwhith the correct file paths.

The Unix style 'make' command must be present too.

Type 'make' and let the build process proceed.

Once the libs are compiled move them (*.lib) in {z88dk}/lib/clibs.

Happy coding !

# (Re)building the z88dk libraries on windows

The following is a recipe for building the libraries from a vanilla machine, you will probably need adjust if you've already got a source tree.

1. Open git-bash
2. git clone https://www.github.com/z88dk/z88dk
3. Download the latest build from http://nightly.z88dk.org
4. Copy the bin folder from the nightly kit into the cloned tree
5. Download make-4.1-2-without-guile-w32-bin.zip (get the version without guile) from https://sourceforge.net/projects/ezwinports/files/
6. Open git-bash in administrator mode
7. cd /
8. unzip ~/Downloads/make-4.1-2-without-guile-w32-bin.zip
9. Open git-bash as a normal user
10. cd z88dk/
11. export PATH=`pwd`/bin:$PATH
12. export ZCCCFG=`pwd`/lib/config
13. cd libsrc
14. make


