To build software with EasyBuild, the first things you'll need to do is either pick a supported compiler toolchain, or define your own and make EasyBuild support it.

Compiler toolchains are basically a (set of) compilers together with a bunch of libraries that provide additional support that is commonly required. In the world of HPC where EasyBuild was born, this usually consists of an library for MPI (inter-process communication over a network), BLAS/LAPACK (linear algebra routines), FFT (Fast Fourier Transforms), etc.

(to be continued)

## List of supported compiler toolchains

## Creating a new compiler toolchains

### Creating a module for the toolchain

### Making EasyBuild support the module

