To build software with EasyBuild, the first thing you'll need to do is either pick a supported compiler toolchain, or construct your own and make EasyBuild support it.

Compiler toolchains are basically a (set of) compilers together with a bunch of libraries that provide additional support that is commonly required to build software.
In the world of High Performance Computing where EasyBuild was born, this usually consists of an library for MPI (inter-process communication over a network), BLAS/LAPACK (linear algebra routines) and FFT (Fast Fourier Transforms).

The following sections describe how to either pick an already supported compiler toolchain, or how to construct your own toolchain and provide the necessary things so that EasyBuild can use it.

## Select one of the supported compiler toolchains

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

For each compiler toolchain, the constituent elements (compiler + libraries) are printed, which provides the necessary information to select a toolchain. To select a version of a particular toolchain, just check which modules are available for the toolchain you selected (e.g., `module av goalf`), or build a module featuring the versions of the compiler and libraries you need.

If none of these toolchains fits your needs, you will need to construct your own compiler toolchain.

## Create a new compiler toolchain

EasyBuild has very modular support for compiler toolchains, making it very easy to construct your own toolchain and make EasyBuild use it.

### Create your own EasyBuild toolchains package

### Provide support for the toolchain

### Create a module for the toolchain

The first step is to build a module for the toolchain you will be using. When instructed to use a particular toolchain, EasyBuild will try and load the corresponding module to make the compiler and libraries available for use.

First, you'll need to make sure that all of the constituent elements of the toolchain are supported by EasyBuild, i.e. that you can build and install them, and thus create a module that can be loaded. 

Assuming that that is taken care of you create a simple easyconfig file to make EasyBuild create a module for your toolchain, the most important part being the list of dependencies. For example, for the goalf toolchain the easyconfig file looks like this:

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

Note that to 'build' a toolchain module, you should use the `dummy` toolchain (since you won't actually be building anything, just creating a module file).