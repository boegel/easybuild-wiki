* [Introduction](#intro)
* [Step 1: Compilers](#step1)
* [Step 2: MPI library](#step2)
* [Step 3: BLAS and LAPACK libraries](#step3)
* [Step 4: FFTW library](#step4)
* [Step 5: ScaLAPACK library](#step5)
* [Step 6: Compiler toolchain](#step6)
* [Step 7: Building software with compiler toolchain](#step7)
* [Step 8: Updating the compiler: automatic dependency resolution](#step8)

***

<a name="wiki-intro"/>
## Introduction

This step-by-step guide will guide you through putting together a self-contained compiler toolchain, and using that toolchain to build a software package.
It is assumed you have already [[configured|Configuration]] EasyBuild.

**Note, this guide is here to try to inform you about what is going on behind the scenes in EasyBuild, if you want to get started quickly, take an easyconfig from https://github.com/hpcugent/easybuild/wiki/List-of-supported-software-packages**
**and run it with `eb software-version-toolchain-toolchainversion.eb --robot`**
**This will enable the robot optiand which will automatically do everything explained below.**

For more information on what a compiler toolchain is and why EasyBuild uses them, see the [[Compiler toolchains]] wiki page.

In this guide, we will put together the `goalf` toolchain, which consists of:

* the [[GNU Compiler Collection (GCC)|http://gcc.gnu.org/]], a set of open-source C/C++/Fortran compilers named gcc, g++ and gfortran,
* the [[OpenMPI|http://www.open-mpi.org/]] library, which provides support for building MPI (Message Passing Interface) applications,
* the [[ATLAS|http://math-atlas.sourceforge.net/]] and [[LAPACK|http://www.netlib.org/lapack/]] libraries, which provide highly tuned linear algebra routines,
* the [[FFTW|http://www.fftw.org/]] library, which provides fast discrete Fourier transform routines,
* the [[ScaLAPACK|http://www.netlib.org/scalapack/]] library, which provides MPI-enabled LAPACK routines.

Note that the name we give to the toolchain is arbitrary; you might as well name it `myToolkit`.

The build times specified for each of the toolchain components below are estimates, based on the build times observed on an otherwise unloaded Dell E6420 laptop
with a quad-core Intel Core i5 2.4GHz processor and 8GB of physical memory, running 64-bit Fedora 16 Linux with a 3.x kernel.

_Note_: the various .eb easyconfigs mentioned below are also available in the EasyBuild package, see the `easybuild/easyconfigs` subdirectory,
and are also used by the [[quick demo|https://github.com/hpcugent/easybuild/wiki/Quick-demo-for-the-impatient]].

<a name="wiki-step1"/>
## Step 1: Compilers

The first step is to build the set of compilers that will be used in our toolchain. They are used further on for building the libraries that make up our toolchain
and for building the software packages we wish to deploy on our system.

We aim to make this entire process as self-contained as possible, by reducing the dependencies on existing (external) system libraries.
This is important for compilers that already are part of a toolchain, as they most often rely on the presence of certain tools in the system.

GCC supports a so-called _bootstrap build_, in which the compiler is built in 3 stages:
once using the system compiler, once with the compiler obtained from stage 1, and finally using the compiler from stage 2.

This bootstrap build procedure is implemented by the GCC easyblock that comes with EasyBuild, so you do not need to worry about it.

Simply provide EasyBuild with an easyconfig that describes which GCC version you wish to build, and you will obtain a self-contained GCC build.

### Step 1.1 Create easyconfig for GCC

Create an easyconfig file `GCC-4.7.2.eb` with the following contents:

```python
name = "GCC"
version = '4.7.2'

homepage = 'http://gcc.gnu.org/'
description = "The GNU Compiler Collection includes front ends for C, C++, Objective-C, Fortran,
               Java, and Ada, as well as libraries for these languages (libstdc++, libgcj,...)."

toolchain = {'name':'dummy', 'version': 'dummy'}

sources = ['%s-%s.tar.gz' % (name.lower(), version),
           'gmp-5.0.4.tar.bz2',
           'mpfr-3.0.1.tar.gz',
           'mpc-0.9.tar.gz',
          ]
source_urls = ['http://ftpmirror.gnu.org/%(name)s/%(name)s-%(version)s' %
                {'name': name.lower(), 'version': version}, # GCC auto-resolving HTTP mirror
              'http://ftpmirror.gnu.org/gmp', # idem for GMP
              'http://ftpmirror.gnu.org/mpfr', # idem for MPFR
              'http://www.multiprecision.org/mpc/download', # MPC official
             ]
languages = ['c','c++','fortran','lto']

moduleclass = 'compiler'

# building GCC sometimes fails if make parallelism is too high, so we limit it
maxparallel = 4
```

Most of entries in the easyconfig should be clear (if not, see [[Easyconfig files]]).

Some remarks:

* The dummy toolchain specifies that we use the system compiler to kickstart the GCC bootstrap build. However, after completing the build procedure, the resulting GCC will be self-contained because it did not use the system compiler in the final build stage.

* EasyBuild will try and download the requires source files given the paths in source_urls, unless they are available in the source path already.

* The `languages` config entry is specific to the GCC easyblock, and specifies for which programming languages support should be enabled.

* Because of problems with parallel building for some of the libraries on which GCC depends, we limit parallel building to a maximum of 4 simultaneous processes.
EasyBuild auto-detects how many cores you have in your system, and will build in parallel accordingly (unless instructed otherwise).


### Step 1.2 Build and install GCC

Instruct EasyBuild to build GCC by providing it the easyconfig:

    eb GCC-4.7.2.eb

Building GCC v4.7.2 with a bootstrap build as performed by the GCC easyblock takes about 35 minutes (on the aforementioned system).


### Step 1.3 Verify installation

Once the installation is complete, you should see a message like `COMPLETED: Installation ended successfully` appearing, both on the command line output
and in the log file created by EasyBuild.

To verify that EasyBuild has produced a working GCC build, load the `GCC/4.7.2` module provided and check the version:

    module load GCC/4.7.2
    gcc --version | grep ^gcc

This should yield something like `gcc (GCC) 4.6.3` as output.


<a name="wiki-step2"/>
## Step 2: MPI library

The next step is to build **OpenMPI**, that will serve as MPI library in the `goalf` compiler toolchain.

Again, just create an easyconfig and build/install OpenMPI using EasyBuild.


### Step 2.1: Create easyconfig for OpenMPI

Create an easyconfig `OpenMPI-1.4.5-no-OFED.eb` with the following contents:

```python
name = 'OpenMPI'
version = '1.6.4'
versionsuffix = "-no-OFED"  # no InfiniBand support, so add version suffix

homepage = 'http://www.open-mpi.org/'
description = "The Open MPI Project is an open source MPI-2 implementation."

toolchain = {'name': 'GCC','version': '4.7.2'}

sources = ['%s-%s.tar.gz'%(name.lower(),version)]
source_urls = ['http://www.open-mpi.org/software/ompi/v%s/downloads' % '.'.join(version.split('.')[0:2])]

configopts = '--with-threads=posix --enable-shared '

moduleclass = 'lib'

sanity_check_paths = {
                    'files':["bin/%s" % binfile for binfile in ["ompi_info", "opal_wrapper", "orterun"]] +
                            ["lib/lib%s.so" % libfile for libfile in
                                     ["mca_common_sm", "mpi_cxx", "mpi_f77" ,"mpi_f90",
                                      "mpi", "openmpi_malloc", "open-pal", "open-rte"]],
                    'dirs':["include/openmpi/ompi/mpi/cxx"]
                   }
```

Some specific remarks with regard to this easyconfig:

* EasyBuild adds a version suffix `-no-OFED` for this build, because we do not enable InfiniBand (IB) support (using the OFED stack) in this OpenMPI build procedure.
For the sake of this demo, we do not bother with installing required dependencies that allow enabling IB support.

* We use the GCC built in the previous step as a toolchain for building OpenMPI, as specified by the `toolchain` entry in the easyconfig.

* OpenMPI follows the more-or-less standard `configure`/`make`/`make install` build procedure. Hence there is no real need for a dedicated OpenMPI easyblock.
This means that EasyBuild will instead fall back to the default ConfigureMake easyblock that implements this `configure`/`make`/`make install` build procedure.

* Non-default configure options are specified using the `configopts` entry. Here, we detail which files and directories are expected to be installed using the
`sanity_check_paths` config entry.


### Step 1.2 Build and install OpenMPI

Instruct EasyBuild to build OpenMPI by providing it with the easyconfig:

```bash
eb OpenMPI-1.6.4-no-OFED.eb
```

Building and installing OpenMPI should only take about 7 minutes.


<a name="wiki-step3"/>
## Step 3: BLAS and LAPACK libraries

### Step 3.1: Building/install LAPACK

*** Step 3.1.2: Install gompi compiler toolchain (see https://github.com/hpcugent/easybuild-easyconfigs/tree/master/easybuild/easyconfigs/g/gompi for example easyconfigs, and further down this guide to explain how to create your own toolchain easyconfigs.

`eb  gompi-1.4.10.eb`

**Step 3.1.1: Create easyconfig for LAPACK**

```python
name = 'LAPACK'
version = '3.4.2'

homepage = 'http://www.netlib.org/lapack/'
description = """LAPACK is written in Fortran90 and provides routines for solving systems of simultaneous linear equations,
least-squares solutions of linear systems of equations, eigenvalue problems, and singular value problems."""

toolchain = {'name':'gompi','version':'1.4.10'}
toolchainopts = {'pic':True}

sources = ['%s-%s.tgz'%(name.lower(),version)]
source_urls = [homepage]

moduleclass = 'lib'
```


**Step 3.1.3: Build and install LAPACK**
`eb  LAPACK-3.4.2-gompi-1.4.10.eb`



### Step 3.3: Building/installing OpenBLAS

**Step 3.3.1: Create easyconfig for OpenBLAS**
* Use this easyconfig: https://github.com/hpcugent/easybuild-easyconfigs/blob/master/easybuild/easyconfigs/o/OpenBLAS/OpenBLAS-0.2.6-gompi-1.4.10-LAPACK-3.4.2.eb
* witch will need this patch https://github.com/hpcugent/easybuild-easyconfigs/blob/master/easybuild/easyconfigs/o/OpenBLAS/OpenBLAS-0.2.6_Makefile-LAPACK-sources.patch


**Step 3.3.2: Build and install ATLAS**
`eb OpenBLAS-0.2.6-gompi-1.4.10-LAPACK-3.4.2.eb`

<a name="wiki-step4"/>
## Step 4: FFTW library

### Step 4.1: Create easyconfig for FFTW

```python
name = 'FFTW'
version = '3.3.3'

homepage = 'http://www.fftw.org'
descripti on= """FFTW is a C subroutine library for computing the discrete Fourier transform (DFT)
in one or more dimensions, of arbitrary input size, and of both real and complex data."""

toolchain = {'name': 'gompi', 'version': '1.4.10'}
toolchainopts = {'optarch': True, 'pic': True}

sources = ['%s-%s.tar.gz' % (name.lower(), version)]
source_urls = [homepage]

versionsuffix = '-%s-%s%s' % (mpilib, mpiver, mpisuff)

configopts = "--enable-sse2 "

## the MPI opts from FFTW2 are valid options but unused until FFTW3.3
configopts += "--with-openmp --with-pic --enable-mpi "

moduleclass = 'lib'
```

### Step 4.2: Build and install FFTW
`eb fftw.eb`



<a name="wiki-step5"/>
## Step 5: ScaLAPACK library

### Step 5.1: Create easyconfig for ScaLAPACK

```python
name = 'ScaLAPACK'
version = '2.0.2'

homepage = 'http://www.netlib.org/scalapack/'
description = """The ScaLAPACK (or Scalable LAPACK) library includes a subset of LAPACK routines
redesigned for distributed memory MIMD parallel computers."""

toolchain = {'name': 'gompi', 'version': '1.4.10'}
toolchainopts = {'pic': True}

source_urls = [homepage]
sources = ['%(namelower)s-%(version)s.tgz']

blaslib = 'OpenBLAS'
blasver = '0.2.6'
blassuff = '-LAPACK-3.4.2'

versionsuffix = "-%s-%s%s" % (blaslib, blasver, blassuff)

dependencies = [
    (blaslib, blasver, blassuff),
]

## parallel build tends to fail, so disabling it
parallel = 1

moduleclass = 'numlib'
```

### Step 5.2: Build and install ScaLAPACK




<a name="wiki-step6"/>
## Step 6: Compiler toolchain

### Step 6.1: Create easyconfig for goalf compiler toolchain

```python
easyblock = "Toolchain"

name = 'goolf'
version = '1.4.10'

homepage = '(none)'
description = """GNU Compiler Collection (GCC) based compiler toolchain, including
 OpenMPI for MPI support, OpenBLAS (BLAS and LAPACK support), FFTW and ScaLAPACK."""

toolchain = {'name': 'dummy', 'version': 'dummy'}

comp_name = 'GCC'
comp_version = '4.7.2'
comp = "%s-%s" % (comp_name, comp_version)

blaslib = 'OpenBLAS'
blasver = '0.2.6'
blas = '%s-%s' % (blaslib, blasver)
blassuff = 'LAPACK-3.4.2'

# toolchain used to build goolf dependencies
comp_mpi_tc_name = 'gompi'
comp_mpi_tc_ver = "%s" % version
comp_mpi_tc = "%s-%s" % (comp_mpi_tc_name, comp_mpi_tc_ver)

# compiler toolchain depencies
# we need GCC and OpenMPI as explicit dependencies instead of gompi toolchain
# because of toolchain preperation functions
dependencies = [
                ('GCC', '4.7.2'),
                ('OpenMPI', '1.6.4-%s' % comp),  # part of gompi-1.1.0
                (blaslib, blasver, '-%s-%s' % (comp_mpi_tc, blassuff)),
                ('FFTW', '3.3.3', "-%s" % comp_mpi_tc),
                ('ScaLAPACK', '2.0.2', '-%s-%s-%s' % (comp_mpi_tc, blas, blassuff))
               ]

moduleclass = 'toolchain'
```

### Step 6.2: Install goolf compiler toolchain

`eb  goolf-1.4.10.eb`


<a name="wiki-step7"/>
## Step 7: Building software with compiler toolchain

(more soon)



<a name="wiki-step8"/>
## Step 8: Updating the compiler: automatic dependency resolution

### Step 8.1: Create easyconfigs

### Step 8.1: Install new compiler toolchain using robot for automatic dependency resolution