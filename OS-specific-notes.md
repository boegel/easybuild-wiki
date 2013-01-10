## Ubuntu 10.04

* install tcl8.5 and tcl8.5-dev (apt-get and friends)
* download environment-modules and `configure`/`make`/`sudo make install`

After this (with a working g++), the demo procedure pretty much worked out of the box (except for a minor issue with a Python module loading, but that's beside the point).

## Ubuntu 11.10

* install Tcl and environment-modules:
 * `sudo apt-get install tcl8.5-dev`
 * download environment-modules and `configure`/`make`/`sudo make install`
 * add default install path to PATH
```
export PATH=/usr/local/Modules/3.2.9/bin:$PATH
```
* install `g++` (only `gcc` is installed by default)
```
sudo apt-get install g++
```
* install 32-bit libc: 
 * fixes missing `bits/predefs.h` error message
```
sudo apt-get install libc6-dev-i386
```
* adjust `LIBRARY_PATH` to fix `/usr/bin/ld: cannot find crti.o: No such file or directory`
```
export LIBRARY_PATH=/usr/lib/x86_64-linux-gnu
```
 * this (maybe) also needs to set when using the built GCC compilers

## Arch Linux

* install Tcl, Python 2.x and make: 
```
pacman -S tcl python2 make
```
* download and `configure`/`make`/`make install` environment-modules

## OSX 10.7.4

* Tcl is installed, but you may have to disable X when building the environment-modules (configure --without-x)
* can build GCC,OpenMPI and Lapack but fails to build ATLAS