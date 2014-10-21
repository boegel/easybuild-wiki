EasyBuild has a couple of dependencies, some of them optional:

Required
--------

 * Linux or OS X operating system
 * [[Python 2.4|http://python.org/]], or a more recent 2.x version
 * [[environment-modules|http://modules.sourceforge.net/]], version >= 3.2.10
  * a packaged version is available for [[RPM-based systems|https://rhn.redhat.com/errata/RHBA-2014-0327.html]] and [[Debian/Ubuntu|https://packages.debian.org/testing/main/environment-modules]].
  * environment-modules requires [[Tcl|http://www.tcl.tk/]] to be installed (with header files and development libraries)
  * a guide on installing Tcl and environment without having root permissions is available [[here|Installing environment modules without root permissions]]
 * a C/C++ compiler

EasyBuild is written in Python, so a Python installation is indispensable.

EasyBuild not only generates module files to be used along with the software it installs, it also depends on the generated modules for some of its functionality. In practice, you need environment modules to make full use of EasyBuild's features.

The C/C++ compiler is only required when an open-source compiler will be used to build software applications. EasyBuild will construct a GCC compiler toolchain first, before building the software applications, and to build the compiler to be part of the toolchain from source typically a C/C++ (system) compiler is required.

### Python modules

 * (none)

## Optional

### Python modules

 * [[GitPython|http://gitorious.org/git-python]], only needed if EasyBuild is hosted in a git repository or if you're using a git repository for easyconfig files (.eb)
 * [[pysvn|http://pysvn.tigris.org/]], only needed if you're using an SVN repository for easyconfig files (.eb)