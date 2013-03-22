(Tuesday Mar. 12th 2013, 10am-6pm)

The second day of the [[3rd EasyBuild hackathon]] was the first day of the actual 2-day hackathon. The attendees got to know EasyBuild better by installing it and getting hands-on experience on building software. Some already even got to adding support for new software by producing their own easyconfig files, or started fixing outdated easyblocks/easyconfigs for software they care about.

These notes were mainly taken by Kenneth and Jens, with contributions by Fotis.

## Attendees

 * Kenneth Hoste (HPC-UGent, EasyBuild developer and release manager)
 * Jens Timmerman (HPC-UGent, EasyBuild developer)
 * Fotis Georgatos (University of Luxembourg, HPC sysadmin and active contributor)
 * Thekla Loizou (The Cyprus Institute, HPC user support)
 * George Tsouloupas (The Cyprus Institute, HPC sysadmin and user support)
 * Mohamed Gafaar (Bibliotheca Alexandrina, HPC sysadmin/user support)
 * Dina Mahmoud Ibrahim (Cairo University, HPC sysadmin/user support)
 * Alan O'Cais (Jülich Supercomputing Centre, HPC user support & LinkSCEEM)
 * Alexander Schnurpfeil (Jülich Supercomputing Center, HPC user suppot)
 * Nicolas Kanaris (The Cyprus Institute, HPC user (OpenFOAM))
 * George Fanourgakis (The Cyprus Institute, HPC user, molecular dynamics)
 * Stelios Erotokritou (The Cyprus Institute, HPC user support/PRACE)

## Program

 * [10am-6pm] 1st day of actual hackathon
 * [6pm-8.00pm] aftermath: discussathon with George T., Fotis, Jens T. and Kenneth

## Discussion notes

 * [Fotis]
  * discuss a way to make modules conflict for compilers, MPI, BLAS/LAPACK, ...(?)
   * ie. it makes not much sense to have 2 different MPI stacks loaded together, better spit a warning
   * KH?: one way would be to add ghost modules being loaded as e.g. a compiler is needed
   * `COMPILER/GCC-4.6.3`
   * add extra `conflict COMPILER` line in GCC, icc, ifort modules
   * problems:
    * shows up in `module list`
    * `icc` vs `ifort` needs special attention, they're both compilers
     * `C_COMPILER` vs `FORTRAN_COMPILER`?
    * no way of easily combining GCC and Intel compilers anymore if you wanted to

## Hackathon notes

 * George F.: Ferret
  * ported outdated easyblock and easyconfig for Ferret
  * patch out hardcoded from *.mk file used by Ferret
  * add missing dependencies in easyconfig (zlib, ncurses, cURL, ...)
 * Nicolas
  * OpenFOAM / pyFOAM
  * look into OpenFOAM 'extensions' (commercial ThirdParty stuff)
 * Alexander
  * getting EasyBuild to work on JUROPA (SLES/x86-64)
   * with Tcl environment modules
  * interested in building NWChem
 * Dina
  * work on training exercises and Getting Started wiki page (gzip, ...)
  * build GCC in VM on her laptop
  * build OpenMPI with dummy toolchain during waiting
 * Alan
  * building basic libraries
  * look into UNITE and its dependencies
 * George & Fotis
  * CUDA toolkit 5.0.35; DONE.
  * GROMACS v4.6 (w/o CUDA); DONE.
  * FFTW 3.3.3; DONE.
  * OpenMPI v1.7 for DMA to GPU support
   * only 1.7rc8 is available; DONE.
   * Fortran libraries were renamed in this version
    * `libmpi_f77` -> `libmpi_mpifh`
    * `libmpi_f90` -> `libmpi_usempi`
   * requires GCC **newer** than v4.8 (which hasn't been released yet)
    * see http://www.open-mpi.org/software/ompi/versions/ (bottom of page)
```
The incorrect interface was removed in Open MPI v1.7.

To be clear: applications that use the old/incorrect MPI_SCATTERV
binding will no longer be able to compile properly (*).  Developers
must fix their applications or use an older version of Open MPI.

(*) Note that using this incorrect MPI_SCATTERV interface will not be
    recongized in v1.7 if you are using gfortran (as of gfortran
    v4.8).

    This is because gfortran <=v4.8 does not (yet) have the support
    Open MPI needs for its new, full-featured "mpi" and "mpi_f08"
    modules.  Hence, Open MPI falls back to the same "mpi" module from
    the v1.6 series, but the "large" size of that module -- which
    contains the MPI_SCATTERV interface -- been disabled because it is
    broken.  Further, this "large" sized (old) "mpi" module has been
    deemed unworthy of fixing because it has been wholly replaced by a
    new, full-featured "mpi" module.  We anticipate supporting
    gfortran in the new, full-featured module in the future.
```
  * OpenMPI v1.6.3 & v1.6.4 (Fotis); DONE.
   * with slightly more relaxed sanity check paths
  * goolf 1.4.10; ~DONE (deviations: FFTW is the default version, so far)
   * GCC 4.7.2
   * OpenMPI 1.6.4
   * OpenBLAS 0.2.6
   * LAPACK 3.4.2
   * FFTW 3.3.3 (single/double)
   * ScaLAPACK 2.0.2
  * goolf 1.5.10; same as above, except with OpenMPI v1.7rc8; existing just as a prototype really
  * gmolf: no much progress on that yet
  * goolfc
   * CUDA toolkit 5.0.35
  * GROMACS v4.6 w/ CUDA support
 * Jens T.
  * help out with several people
 * Kenneth
  * write easyconfig for OpenBLAS
  * help out with several people