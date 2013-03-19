Frequently asked questions.

***

## Installation problems

### error: Couldn't find a setup script in easybuild

If you're seeing a problem ```error: Couldn't find a setup script in easybuild``` when installing EasyBuild with ```easy_install```, you are most likely running the command in a directory which has a subdirectory named `easybuild`. 

In that case, ```easy_install``` is trying to find a `setup.py` script in the `easybuild` directory, while you probably want it to go look for EasyBuild on PyPi.

**Solution:** Run the ```easy_install``` command line in a directory which doesn't have a subdirectory named `easybuild`.

### error: option --user not recognized

The `--user` option for `easy_install` is only supported in recent version of the `setuptools` Python package.

You will need to resort to the `--prefix` option to install EasyBuild in a custom path, and make sure that the expected directory structure is there, and that the `PYTHONPATH` environment variable is set correctly:

```bash
PYLIB=`python -c "import distutils.sysconfig; print distutils.sysconfig.get_python_lib(prefix='/tmp'); "`
mkdir -p $PYLIB
export PYTHONPATH=$PYLIB:$PYTHONPATH
easy_install --prefix=/tmp easybuild
```
#### ERROR: Failed to locate EasyBuild's main script easybuild/main.py

main.py should be in `<prefix>/lib/python2.7/site-packages/easybuild/`
So set your pythonpath to point to e.g.,`.local/lib/python2.7/site-packages/`

Sometimes it is not correctly installed. Older versions of pip install in the right place, but then instantly overwrite the `easybuild` folder by installing the `easyblocks` package.
This might occur when using pip < 1.2.1. Update your pip, and try using the ` --exists-action i` option.

***

## Using EasyBuild

### gnu/stubs-32.h: No such file or directory
You are missing some 32bit/multilib packages, or need to set some extra path to their location.
More is explained here: http://stackoverflow.com/questions/7412548/gnu-stubs-32-h-no-such-file-or-directory



### EasyBuild crashed, and I can't seem to find a log file to figure out what went wrong?

EasyBuild stores a temporary log file in the location determined by:

```bash
python -c "import tempfile; print tempfile.gettempdir()"
```

On Linux systems, this is usually simply `/tmp`.

On Mac OS X, it's more obscure, e.g., `/var/folders/6y/x4gmwgjn5qz63b7ftg4j_40m0000gn/T`. To help you locate the EasyBuild you are looking for on OS X, you could use something like

```bash
find /var/folders/ -name 'easybuild-*' 2> /dev/null
```

### Can I add a dependency on an os specific package (rpm, deb) ##

Yes, but better if you not, they are not really portable:
A better way around is to actually create a .eb file for the dependency
without a sources field.

You can add a sanitycheckcommand to check if the package is available
(it would be nice if this is distribution independent)

This way
* a build will fail without the os dependency available.
* you can later override default tcsh with a custom build one when in
need
* reduce dependencies on OS repositories and distro specific mechanisms
* actually report problems when there are exact version dependencies
(otherwise a default yum/repo update can break a working HPC
application)

### Can I build different numpy and scipy versions? ##

Yes, use the PythonPackageModule to build different version without rebuilding
Python.

### Can I create module files which just load a set of modules? ##

Yes, if you create an easyconfig file with no sources and only dependencies.
You could ofcourse also just create the module file yourself.

### Easybuild complains about an OS dependency, yet I am certain it is installed

Currently easybuild's support for OS dependencies is lacking. It will try to
find it using rpm and dpgk, but this is not perfect.
If you know what you are doing, you can remove the OS dependency from the
easyconfig file, and then build it.

We are trying to remove as much os dependencies as possible, however we can not (yet) build an entire linux system from scratch.

### I need a module for a software package with a certain toolchain, do I really need to rebuild the whole stack of dependencies with that toolchain?

Compiler toolchains as used in EasyBuild are a set of modules that are grouped together under a mnemonic (e.g., `goalf`, `ictce`, `goolf`, ...). This is a lot more convenient than listing all commonly used dependencies in the module name to make them more explicit; consider picking between a module name like `CMake-2.8.7-goolf-1.4.10` or `CMake-2.8.7-GCC-4.7.2-OpenMPI-1.6.4-OpenBLAS-0.2.26-LAPACK-3.8.4-FFTW-3.3.3`.

When you need to build a particular software package with a certain toolchain, e.g., so it can be used alongside other modules that were built with that toolchain, the easiest you can do is just build it. EasyBuild provides different command line option that make this task really easy, e.g., `--robot` for resolving dependencies, and `--try-toolchain` for tweaking the toolchain specified in an easyconfig file.

The downside of this is a large set of modules that are added to the potentially already extensive set of available modules. We feel this is not a problem EasyBuild can solve; it should be tackled by the tools used to interact with the module files, i.e. the _environment modules_ package or alternatives like _lmod_. That being said, we are looking into a proper tool that makes dealing with large amounts of modules more feasible, and will add the necessary features to EasyBuild that are required to enable that tool to work. The most likely viable option we're looking into is to enhance _lmod_.

#### Expert mode: using subtoolchains

Another solution that could help to resolve conflicts that would be circumvented by rebuilding a software package with a new toolchain is to resort to _subtoolchains_ like `GCC` or `gompi`, that only contain a subset of libraries (e.g., no BLAS/LAPACK/FFTW or even MPI).

For example, CMake can be built with a `GCC` toolchain, since it does not require any MPI, BLAS, etc. library to build.
The resulting module can then be combined with modules built with a GCC-based toolchain that use the same GCC version, thus limiting the amount of seemingly needless module on the system.

It does require to specify CMake as a dependency in a particular way because the toolchains are not an exact match, e.g.:

```python
name = 'foo'
version = '1.2.3'

toolchain = {'name': 'goolf', 'version': '1.4.10'}
builddependencies = [('CMake', '2.8.7', '-GCC-4.7.2', True)]
```

**We do not recommend using these subtoolchains, because they cause more involved bookkeeping of dependencies,
and may do more harm than good w.r.t. keeping the list of available modules understandable.**
Subtoolchains like `GCC`, `gompi`, `iccifort`, etc. are only meant to be used when constructing toolchains.

***

## Developing and contributing

### How do I contribute my own easyconfig files? ##

If you want your own easyconfig files to be included with easybuild, you must
fork the [easyconfigs repo](http://github.com/hpcugent/easybuild-easyconfigs/) and send a pull
request.  Note: pull requests should be send to the `develop` branch instead of
the `master` branch.