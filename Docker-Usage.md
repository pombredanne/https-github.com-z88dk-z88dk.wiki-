# z88dk Dockerfile

With the provided Dockerfile you are able to build and run z88dk in a [Docker](https://www.docker.com/) container. Try it out!

## About Docker

https://www.docker.com/what-docker

### System requirements

* Docker for Windows runs on 64bit Windows 10 Pro, Enterprise and Education (1607 Anniversary Update, Build 14393 or later).
* Docker for Mac requires OS X El Capitan 10.11 or newer macOS release running on a 2010 or newer Mac, with Intel’s hardware support for MMU virtualization. The app runs on 10.10.3 Yosemite, but with limited support.
* Docker for Ubuntu, Debian, CentOS or Redhat need the 64-bit version of the OS.

### FAQ

Q: What is a Dockerfile, Image, Container?
A: Dockerfile is a textfile containing all the commands, in order, needed to build a given image. Docker image is created by building a Dockerfile using ‘docker build’ command. Docker container is provisioned when you run a docker image using  ‘docker run’ command.

## Install Docker

See installation instructions of docker: https://docs.docker.com/installation/

## Build the image

Just get the file **z88dk.Dockerfile** from the GIT repository and build the docker image "_z88dk_":

```bash
docker build -t z88dk - < z88dk.Dockerfile
```

Note: an Internet connection is required because several packages and source code repositories need to be downloaded.

## Run the container

Simply execute this line in the directory you want z88dk executables to be run:

```bash
docker run -v ${PWD}:/src/ -it z88dk <command>
```

where \<command\> is the command line to be executed in the Docker container.

As it can be deduced, current directory (and its subfolders) contents are binded with read-write access to the Docker container /src/ directory.

Note: \<command\> is not tied to run z88dk executables only: it can run scripts or execute makefiles as long as they are supported by the Docker container. See the examples below...

### Different examples to run the docker container "z88dk":

Compiling all the programs located in the z88dk GIT's examples folder:
```bash
cd ${z88dk}/examples
docker run -v ${PWD}:/src/ -it z88dk make
```

Compiling BlackStar SP1 example with `sccz80`:
```bash
cd ${z88dk}/libsrc/_DEVELOPMENT/EXAMPLES/zx/demo_sp1/BlackStar/
docker run -v ${PWD}:/src/ -it z88dk zcc +zx -v -startup=31 -DWFRAMES=3 -clib=new -O3 @zproject.lst -o blackstar -pragma-include:zpragma.inc
docker run -v ${PWD}:/src/ -it z88dk appmake +zx -b loading.scr -o screen.tap --blockname screen --org 16384 --noloader
docker run -v ${PWD}:/src/ -it z88dk appmake +zx -b blackstar_CODE.bin -o game.tap --blockname game --org 25124 --noloader
docker run -v ${PWD}:/src/ -it z88dk cat loader.tap screen.tap game.tap > blackstar.tap
```

Compiling BlackStar SP1 example with `zsdcc`:
```bash
cd ${z88dk}/libsrc/_DEVELOPMENT/EXAMPLES/zx/demo_sp1/BlackStar/
docker run -v ${PWD}:/src/ -it z88dk zcc +zx -v -startup=31 -DWFRAMES=3 -clib=sdcc_iy -SO3 --max-allocs-per-node200000 --fsigned-char @zproject.lst -o blackstar -pragma-include:zpragma.inc
docker run -v ${PWD}:/src/ -it z88dk appmake +zx -b loading.scr -o screen.tap --blockname screen --org 16384 --noloader
docker run -v ${PWD}:/src/ -it z88dk appmake +zx -b blackstar_CODE.bin -o game.tap --blockname game --org 25124 --noloader
docker run -v ${PWD}:/src/ -it z88dk cat loader.tap screen.tap game.tap > blackstar.tap
```

Compiling Tritone example with provided `build.sh`:
```bash
cd ${z88dk}/libsrc/_DEVELOPMENT/EXAMPLES/zx/demo_tritone/
docker run -v ${PWD}:/src/ -it z88dk ./build.sh
```

### Troubleshooting

* If using Docker for Windows, **Windows PowerShell** shall be used to run Docker commands.
* Because the provided directory (and its subdirectories) are jailed, you cannot access upper directories from ${PWD} inside the Docker container.
i.e., this will fail:
```bash
cd ${z88dk}/examples/spectrum/
docker run -v ${PWD}:/src/ -it z88dk make
```
with the error:
```bash
make: *** No rule to make target '../clisp/clisp.c', needed by 'clisp.tap'.  Stop.
```
To solve this, a workaround is to go up a directory and let `make` compile inside the `spectrum` folder:
```bash
cd ${z88dk}/examples/
docker run -v ${PWD}:/src/ -it z88dk make -C spectrum
```