To build software with EasyBuild, the first thing you'll need to do is either pick a supported compiler toolchain, or construct your own and make EasyBuild support it.

Compiler toolchains are basically a (set of) compilers together with a bunch of libraries that provide additional support that is commonly required to build software.
In the world of High Performance Computing where EasyBuild was born, this usually consists of an library for MPI (inter-process communication over a network), BLAS/LAPACK (linear algebra routines) and FFT (Fast Fourier Transforms).

The following sections describe how to either pick an already supported compiler toolchain, or how to construct your own toolchain and provide the necessary things so that EasyBuild can use it.

# Select one of the supported compiler toolchains

To get an overview of compiler toolchains that EasyBuild knows about, use the `--list-toolchains` command line option (available since EasyBuild v1.1). This will print something like below:

```bash
$ ./eb --list-toolchains
List of known toolchains:
	GCC: GCC
	dummy: 
	gimkl: GCC, imkl, impi
	gmacml: ACML, BLACS, FFTW, GCC, MVAPICH2, ScaLAPACK
	gmpich2: GCC, MPICH2
	gmvapich2: GCC, MVAPICH2
	goalf: ATLAS, BLACS, FFTW, GCC, OpenMPI, ScaLAPACK
	gompi: GCC, OpenMPI
	gqacml: ACML, BLACS, FFTW, GCC, QLogicMPI, ScaLAPACK
	iccifort: icc, ifort
	ictce: icc, ifort, imkl, impi
	iomkl: OpenMPI, icc, ifort, imkl
	ismkl: MPICH2, icc, ifort, imkl
```

For each compiler toolchain, the constituent elements (compiler + libraries) are printed, which provides the necessary information to select a toolchain. To select a version of a particular toolchain, just check which environment modules are available for the toolchain you selected (e.g., `module av goalf`), or build a toolchain environment module (using EasyBuild) featuring the versions of the compiler and libraries you need.

If none of these toolchains fits your needs, you will need to construct your own compiler toolchain.

# Create a new compiler toolchain

EasyBuild has very modular support for compiler toolchains, making it very easy to construct your own toolchain and make EasyBuild use it.

## Create your own EasyBuild toolchains package

Before you create your own compiler toolchain, you need to set up your own `easybuild.toolchains` package in which you can implemented the required Python module that will provide support for your toolchain. Of course, **you will only need to do this once** (for every `easybuild.toolchains` package).

To set up your `easybuild.toolchains` package in a directory `$PREFIX`, run the following commands:

```shell
for subdir in '' compiler fft linalg mpi;
do
  mkdir -p $PREFIX/easybuild/toolchains/$subdir
  touch  $PREFIX/easybuild/toolchains/$subdir/__init__.py
done
touch easybuild/__init__.py
```

This will create an empty `easybuild.toolchains` package, and also initialize the `compiler`, `fft`, `linalg` and `mpi` subpackages. 

## Implement support for the toolchain

To make EasyBuild support your toolchain, you will need to provide Python modules that implement that support. If all of the constituent toolchain element already have Python modules available in the `easybuild.toolchains` subpackages, you'll only need to provide a Python module that defines your compiler toolchain.

### Python modules for toolchain elements

For each of the toolchain elements, i.e. compiler suite and each one of the libraries, a Python module should be available in the appropriate `easybuild.toolchains` subpackage.

For a compiler suite (e.g. GCC or the Intel compilers), this consists of implementing a Python module in `easybuild.toolchains.compiler` that specifies compiler commands, compiler options (both common and unique options), etc. 

For libraries the support maybe fairly concise, e.g. only consisting of listing library files and/or minor customizations of the general support provided by EasyBuild (see the `easybuild.tools.toolchain` package), or rather involved to yield different settings depending on the version of the compiler used in the toolchain (see e.g. the Intel MKL support in `easybuild.toolchains.linalg.intelmkl.py`).

If you need to implement support for yet unsupported compilers and/or libraries, the best place to start is to look at the already available Python modules in [easybuild.toolchains](https://github.com/hpcugent/easybuild-framework/blob/master/easybuild/toolchains) and the general support in [easybuild.tools.toolchain](https://github.com/hpcugent/easybuild-framework/blob/master/easybuild/tools/toolchain). 

If you need support from the EasyBuild developers, don't hesitate to [open an issue in the easybuild-framework repository](https://github.com/hpcugent/easybuild-framework/issues/new), or [contact us](https://github.com/hpcugent/easybuild/wiki/Contact).

### Python module for toolchain

Next to making sure EasyBuild supports the toolchain elements, you also need to define the toolchain itself, by implementing a very simple Python module in `easybuild.toolchains`. As an example, the toolchain definition for `ictce` 
is shown below (see also [easybuild/toolchains/ictce.py](https://github.com/hpcugent/easybuild-framework/blob/master/easybuild/toolchains/ictce.py)).

```python
from easybuild.toolchains.compiler.inteliccifort import IntelIccIfort
from easybuild.toolchains.fft.intelfftw import IntelFFTW
from easybuild.toolchains.mpi.intelmpi import IntelMPI
from easybuild.toolchains.linalg.intelmkl import IntelMKL


class Ictce(IntelIccIfort, IntelMPI, IntelMKL, IntelFFTW):
    """
    Compiler toolchain with Intel compilers (icc/ifort), Intel MPI,
    Intel Math Kernel Library (MKL) and Intel FFTW wrappers.
    """
    NAME = 'ictce'
```

To add support for a toolchain, it suffices define a class that inherits from all of the the classes implementing support for the toolchain elements, and set a toolchain name as `NAME` (that should match the name of the toolchain environment module, see below). EasyBuild will then take care of the nasty details.

## Create an environment module for the toolchain

You need to build a environment module for the toolchain you will be using. When instructed to use a particular toolchain, EasyBuild will try and load the corresponding environemnt module to make the compiler and libraries available for use.

First, you'll need to make sure that all of the constituent elements of the toolchain are supported by EasyBuild, i.e. that you can build and install them, and generate an environment module that can be loaded. 

Assuming that that is taken care of, you create a simple easyconfig file to make EasyBuild create an environment module for your toolchain, of which the most important part is the list of dependencies. For example, the easyconfig file for the `ictce` toolchain looks like this:

```
easyblock = "Toolchain"

name = 'ictce'
version = '4.0.6'

homepage = 'http://software.intel.com/en-us/intel-cluster-toolchain-compiler/'
description = """Intel Cluster Toolchain Compiler Edition provides Intel C,C++ and fortran compilers, Intel MPI and Intel MKL"""

toolchain = {'name': 'dummy', 'version': 'dummy'}

dependencies = [ 
                ('icc', '2011.6.233'),
                ('ifort', '2011.6.233'),
                ('impi', '4.0.2.003'),
                ('imkl', '10.3.6.233')
               ]
```

Note that to 'build' a toolchain environment module, you should use the `dummy` toolchain (since you won't actually be building anything, just creating an environment module file).

**Important remark**: the list of dependencies in the toolchain environment module should match the list of classes in the toolchain definition as implemented by the Python module in `easybuild.toolchains`. More specifically, the names of the dependency modules should match the `NAME` of the toolchain elements as specified in the `easybuild.toolchains.*` Python classes for them.