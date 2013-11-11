When it comes to specifying toolchains in the easyconfig files for a set of software packages, two approaches are possible.
Both of them have advantanges and disadvantages, so it depends on personal taste or use case to pick either of these.

### Minimal toolchains

Use a toolchain that provides the minimal requirements to build the software package.

e.g. `CMake-2.8.4-GCC-4.7.2`, `CMake-2.8.4-icc-2013.5.192`, ...

#### Advantages

* fewer modules to deal with
* provides flexibility for playing around with different MPI libraries
* ...

#### Disadvantages

* makes rebuilding a whole software stack with a different toolchain more elaborate
* ...

### Full toolchains

Only use full toolchains when building software packages. Never use subtoolchains (e.g. `GCC`, `gompi`, ...), except when constructing full toolchains.

e.g. `CMake-2.8.4-goolf-1.4.10`, `CMake-2.8.4-goalf-1.1.0-no-OFED`, ...

#### Advantages

* no worries about which module built with a subtoolchain is compatible with which other modules
* (usually) a single command to build required module (e.g. using `--try-toolchain`)
* ...

#### Disadvantages

* yields lots of modules to deal with, which potentially provide basically the same tools
* ...
