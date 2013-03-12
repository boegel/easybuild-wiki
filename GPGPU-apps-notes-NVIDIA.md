The notes below show how to build selected applications with CUDA support enabled, and were provided by Adam DeConinck (NVIDIA Corporation).

### GROMACS

```
GROMACS 4.6 - Build with OpenMPI 1.7rc5 and CUDA
================================================

http://www.gromacs.org/

Build commands
--------------

	mkdir gromacs && cd gromacs

	wget ftp://ftp.gromacs.org/pub/gromacs/gromacs-4.6.tar.gz

	tar xzf gromacs-4.6.tar.gz

	mkdir gromacs-build

	module load cmake cuda gcc/4.6.3 fftw openmpi

	CC=mpicc CXX=mpiCC cmake ./gromacs-4.6 -DGMX_OPENMP=ON -DGMX_MPI=ON \
	-DGMX_PREFER_STATIC_LIBS=ON -DCMAKE_BUILD_TYPE=Release \
	-DCMAKE_INSTALL_PREFIX=./gromacs-build

	make install


Other Parameters
----------------

cmake parameters:

* `CUDA_HOST_COMPILER` = which host compiler to use (ie which gcc)
* `CUDA_HOST_COMPILER_OPTIONS`
* `CUDA_NVCC_FLAGS`

Default value for `CUDA_NVCC_FLAGS` for Gromacs 4.6, from examining cmake
output after the fact:

    ./CMakeCache.txt:CUDA_NVCC_FLAGS:STRING=-gencode;arch=compute_20,code=sm_20;-gencode;arch=compute_20,code=sm_21;-gencode;arch=compute_30,code=sm_30;-gencode;arch=compute_30,code=compute_30;-use_fast_math;

So, for example, generating optimized code for K20 (`compute_35,sm_35`) would 
require modifying `CUDA_NVCC_FLAGS`.


Running Gromacs Water benchmark
-------------------------------

Download the water benchmark

	wget ftp://ftp.gromacs.org/pub/tmp/water-clean-input.tar.gz
	tar xzf water-clean-input.tar.gz

For the water/1536 benchmark, initialize the topol.tpr file:

	cd water/1536
	~/gromacs/gromacs-build/bin/grompp_mpi -f pme.mdp

Run the benchmark using GPUs

	mpirun -np $NP -hostfile $HOSTFILE ~/gromacs/gromacs-build/bin/mdrun_mpi \
	-s topol.tpr -npme 0 -resethway -noconfout -nb gpu -nsteps 1000 -v

Run the benchmark using CPU only

	mpirun -np $NP -hostfile $HOSTFILE ~/gromacs/gromacs-build/bin/mdrun_mpi \
	-s topol.tpr -npme 0 -resethway -noconfout -nb cpu -nsteps 1000 -v
```


### HOOMD-Blue

```
HOOMD-Blue 0.11.2
=================

<http://codeblue.umich.edu/hoomd-blue/index.html>

Build notes
-----------

    wget http://codeblue.umich.edu/hoomd-blue/downloads/0.11/hoomd-0.11.2.tar.bz2

    tar xjf hoomd-0.11.2.tar.bz2

    cd hoomd-0.11.2

    mkdir hoomd-build

    module load cuda cmake

    cmake . -DENABLE_CUDA=ON -DENABLE_OPENMP=ON -DCMAKE_BUILD_TYPE=Release \
    -DCMAKE_INSTALL_PREFIX=./hoomd-build -DCUDA_ARCH_LIST="20;30;35"

    make install

Docs: <http://codeblue.umich.edu/hoomd-blue/doc/page_compile_guide.html>
```


### LAMMPS

```
LAMMPS with CUDA and OpenMPI - 6Dec12 version
=============================================

<http://lammps.sandia.gov/>

Build steps
-----------

Download LAMMPS

	tar xzf lammps.tar.gz

	export LAMMPS_DIR=`pwd`/lammps-6Dec12


First we'll build the GPU module.

Check that your cuda module sets `CUDA_HOME`, otherwise set it to your
CUDA directory (i.e. /usr/local/cuda). We will use arch `sm_35` for 
Tesla K20.

	cd $LAMMPS_DIR/lib/gpu

	make -f Makefile.linux clean

	module load cuda openmpi

	make CUDA_HOME=$CUDA_HOME CUDA_ARCH="-arch=sm_35" \
	CUDR_CPP=mpicxx CUDR_OPTS="-O2" -f Makefile.linux


Next we'll build the USER-CUDA module. This is just a different way of adding 
CUDA support, and different LAMMPS models may use either one. Note that it has
a different method for specifying the `CUDA_HOME` and the build 
architecture.

	cd $LAMMPS_DIR/lib/cuda

	make clean

	make CUDA_INSTALL_PATH=$CUDA_HOME cufft=2 precision=2 arch=35

Now we'll set up all the LAMMPS modules correctly.

	cd $LAMMPS_DIR/src

	make clean-all

	make yes-manybody

	make yes-molecule

	make yes-replica

	make yes-kspace

	make yes-asphere

	make yes-gpu

	make yes-user-cuda

If you want to list the modules to be used, run

	make package-status

Set up your LAMMPS makefile and put it in the `$LAMMPS_DIR/src/MAKE` dir. 
It needs to point correctly to your MPI and to 
your FFTW if you're using one (here, we are). See Makefile.CUDA for an 
example. Now we'll get set up and build.

	module load fftw/2.1.5

	make CUDA

After the build you should have `lmp_CUDA` in the src/ directory.
```

* `Makefile.CUDA`:

```
# openmpi = Fedora Core 6, mpic++, OpenMPI-1.1, FFTW2

SHELL = /bin/sh

# ---------------------------------------------------------------------
# compiler/linker settings
# specify flags and libraries needed for your compiler

CC =		mpic++
CCFLAGS =	-O2 \
		-funroll-loops -fstrict-aliasing -Wall -W -Wno-uninitialized
SHFLAGS =	-fPIC
DEPFLAGS =	-M

LINK =		mpic++
LINKFLAGS =	-O
LIB =           -lstdc++
SIZE =		size

ARCHIVE =	ar
ARFLAGS =	-rcsv
SHLIBFLAGS =	-shared

# ---------------------------------------------------------------------
# LAMMPS-specific settings
# specify settings for LAMMPS features you will use
# if you change any -D setting, do full re-compile after "make clean"

# LAMMPS ifdef settings, OPTIONAL
# see possible settings in doc/Section_start.html#2_2 (step 4)

LMP_INC =	-DLAMMPS_GZIP

# MPI library, REQUIRED
# see discussion in doc/Section_start.html#2_2 (step 5)
# can point to dummy MPI library in src/STUBS as in Makefile.serial
# INC = path for mpi.h, MPI compiler settings
# PATH = path for MPI library
# LIB = name of MPI library

MPI_INC =     -I$(MPI_HOME)/include  
MPI_PATH = 
MPI_LIB =	-L$(MPI_HOME)/lib

# FFT library, OPTIONAL
# see discussion in doc/Section_start.html#2_2 (step 6)
# can be left blank to use provided KISS FFT library
# INC = -DFFT setting, e.g. -DFFT_FFTW, FFT compiler settings
# PATH = path for FFT library
# LIB = name of FFT library

FFT_INC =       -DFFT_FFTW -I$(FFTLIB)/include
FFT_PATH = 
FFT_LIB =	-L$(FFTLIB)/lib -lfftw

# JPEG library, OPTIONAL
# see discussion in doc/Section_start.html#2_2 (step 7)
# only needed if -DLAMMPS_JPEG listed with LMP_INC
# INC = path for jpeglib.h
# PATH = path for JPEG library
# LIB = name of JPEG library

JPG_INC =       
JPG_PATH = 	
JPG_LIB =	

# ---------------------------------------------------------------------
# build rules and dependencies
# no need to edit this section

include	Makefile.package.settings
include	Makefile.package

EXTRA_INC = $(LMP_INC) $(PKG_INC) $(MPI_INC) $(FFT_INC) $(JPG_INC) $(PKG_SYSINC)
EXTRA_PATH = $(PKG_PATH) $(MPI_PATH) $(FFT_PATH) $(JPG_PATH) $(PKG_SYSPATH)
EXTRA_LIB = $(PKG_LIB) $(MPI_LIB) $(FFT_LIB) $(JPG_LIB) $(PKG_SYSLIB)

# Link target

$(EXE):	$(OBJ)
	$(LINK) $(LINKFLAGS) $(EXTRA_PATH) $(OBJ) $(EXTRA_LIB) $(LIB) -o $(EXE)
	$(SIZE) $(EXE)

# Library targets

lib:	$(OBJ)
	$(ARCHIVE) $(ARFLAGS) $(EXE) $(OBJ)

shlib:	$(OBJ)
	$(CC) $(CCFLAGS) $(SHFLAGS) $(SHLIBFLAGS) $(EXTRA_PATH) -o $(EXE) \
        $(OBJ) $(EXTRA_LIB) $(LIB)

# Compilation rules

%.o:%.cpp
	$(CC) $(CCFLAGS) $(SHFLAGS) $(EXTRA_INC) -c $<

%.d:%.cpp
	$(CC) $(CCFLAGS) $(EXTRA_INC) $(DEPFLAGS) $< > $@

# Individual dependencies

DEPENDS = $(OBJ:.o=.d)
sinclude $(DEPENDS)
```


### OpenCV

```
OpenCV 2.4.4 with CUDA
======================

<http://opencv.org/>

Build notes
-----------

Download OpenCV from <http://opencv.org/downloads.html>

    tar xjf OpenCV-2.4.4.tar.bz2

    cd OpenCV-2.4.4

    mkdir opencv-build

    module load cmake cuda

    cmake . -DWITH_CUDA=ON -DWITH_CUFFT=ON -DWITH_CUBLAS=ON -DHAVE_OPENMP=YES -DCMAKE_BUILD_TYPE=Release -DCMAKE_INSTALL_PREFIX=./opencv-build/

    make install

And of course any other OpenCV options.
```


### QuantumESPRESSO

```
Quantum Espresso 4.3.2 with GPU support
=======================================

<http://www.quantum-espresso.org/>

Build steps
-----------

	mkdir qe

	wget http://qe-forge.org/gf/download/frsrelease/119/232/espresso-4.3.2-GPU.tar.gz

	tar xzf espresso-4.3.2-GPU.tar.gz

	cd espresso-4.3.2-GPU

We'll use the Intel compiler for QE. If your Intel is not installed in the 
usual /opt/intel you will have to point to the correct MKL with `BLAS_LIBS`.

	module load mpi/intel/13.0/openmpi/1.7 cuda intel/Compiler/13.0

	./configure CC=icc CXX=icpc F77=ifort F90=ifort BLAS_LIBS=$MKLROOT \
	--enable-cuda --enable-openmp --enable-parallel --with-cuda-dir=$CUDA_HOME

	make clean

	make pw
```


### QUDA

```
QUDA 0.4.0
==========

<http://lattice.github.com/quda/>

Build notes
-----------

    wget http://github.com/downloads/lattice/quda/quda-0.4.0.tar.gz

    tar xzf quda-0.4.0.tar.gz

    cd quda-0.4.0

    module load cuda openmpi

    ./configure --prefix=<install-path> --enable-multi-gpu \
    --with-mpi=$MPI_HOME CC=mpicc CXX=mpiCC

    make
    
    make install
```

### Charm++

```
Building Charm++ 6.2.1
======================

    wget http://charm.cs.uiuc.edu/distrib/charm-6.2.1_src.tar.gz
    tar xzf charm-6.2.1_src.tar.gz
    cd charm-6.2

Charm++ has a bunch of different pre-selectable configurations which can be
used, detailed on `http://charm.cs.uiuc.edu/manuals/html/charm++/A.html` . This
includes options for whether or not to use MPI, whether to include Infiniband
support, etc. Note that the `build` script may do some interactive question and
answer. In this case I'll build Charm++ with MPI enabled, but you should choose
your build process based on your cluster's needs.

    module load openmpi
    env MPICXX=mpicxx ./build charm++ mpi-linux-x86_64 --with-production
    ./build charm++ net-linux-x86_64 ibverbs -with-production -j8

Testing the build

    cd mpi-linux-x86_64/tests/charm++/megatest
    make pgm
    mpirun -np 4 ./pgm
```

### NAMD

This build process is known to work on Adam's cluster @ NVIDIA,
but may not be the best performing, etc; it's based in part on
the broken instructions, and in part on Adam his work to get it
to build.

In building NAMD, it's worth spending some time looking at the arguments
to the `./config` file in the source directory, and also looking at the
contents of the `arch/` directory, for information as to what parameters are
available and useful. There's also some documentation on doing a build on
the NAMD homepage at
http://www.ks.uiuc.edu/Research/namd/2.9/notes.html#compiling , but it
appears to be somewhat out of date...

```
NAMD 2.9 with CUDA support
==========================

Download NAMD from http://www.ks.uiuc.edu/Research/namd/ (requires 
registration).

    tar xzf NAMD_2.9_Source.tar.gz
    cd NAMD_2.9_Source
    
NAMD requires tcl, fftw 2.1.5 and Charm++. In this example, I am using
system tcl libraries and an fftw module. See the charm-6.2.markdown file for 
details on building Charm++.

Edit `Make.charm` to set the CHARMBASE parameter to the install path of
Charm++.

Edit `arch/Linux-x86_64.fftw` to set the FFTDIR parameter to the install path
of FFTW. Note that your FFTW has to have been compiled with the 
single-precision option enabled (--enable-float and 
--enable-type-prefix) so you have the sfftw.h include files.

Edit `arch/Linux-x86_64.tcl` to set the TCLDIR parameter to the install path
of TCL. (In this case, using / , and making sure the libdir is correct, since
I'm using the system TCL.) 

For modules, I'm using the same OpenMPI I used to build Charm++, and I'm using
the CUDA module.

    module load cuda openmpi
    ./config Linux-x86_64-g++ --charm-arch mpi-linux-x86_64 --with-cuda --cuda-prefix $CUDA_HOME
    cd Linux-x86_64-g++
    make

This should produce a namd2 binary, as well as a charmrun binary and some other
files.
```