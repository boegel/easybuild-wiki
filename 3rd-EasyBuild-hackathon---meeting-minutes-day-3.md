(Wednesday Mar. 13th 2013, 10am-6pm)

The last day of the [[3rd EasyBuild hackathon]] was a continuation of the hackathon, with more discussions and hands-on experience,
and several contributions being added by the attendees, i.e. pull requests for easyconfig files they were able to get working with EasyBuild.

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
 * Alexander Schnurpfeil (Jülich Supercomputing Center, HPC user suppore
 * George Fanourgakis (The Cyprus Institute, HPC user, molecular dynamics)
 * Stelios Erotokritou (The Cyprus Institute, HPC user support/PRACE)

## Program

 * [10am-6pm] 2nd day of actual hackathon
 * [6pm-9.30pm] aftermath: discussathon with George T., Fotis, Jens T. and Kenneth

## Discussion notes

 * [Alexander/Alan]
  * JUBE (Jeulich Benchmarking Environment)
   * benchmarking environment in Perl
   * plugin for metadata on apps (XML)
   * multiple runs for statistics
   * follow-up focuses performance monitoring on top of that
  * EasyBuild event in Juelich
   * presentation + hands-on w/ focus on certain apps
   * mid-May probably too early
   * problems that were faced with during the hackathon need to be resolved first
    * test accounts can be provided on JUROPA and JUQUEEN
   * pro- or post-ISC? after summer?
  * EasyBuild would be interesting for LinkSCEEM test cluster, and follow-up of JUROPA
   * timing is good for BlueGene Q systems as well, because they're still quite fresh
 * [Fotis/George T.]
  * FFTW single vs double
   * `configure`/`make`/`make install` cycle needs to be run twice for single/double precision FFTW libs (double is default)
   * a dedicated easyblock is required to be able to make a single+double precision FFTW module
    * (temporary) workaround is to define a module for `FFTWsp`, such that it can also be used in a toolchain together with FFTW (double prec.)
   * single precision is required for GROMACS, do we really need a full dedicated toolchain for that (`goolf/1.5.10-sp`)?
   * similar issue with FFTW v2 and v3, which can be combined in the same module (different library names)
  * CUDA integration
   * support for installing CUDA toolkit via easyblock is ready
    * except for handling CUDA samples properly, which introduce an extra os dependency (`libglut.so`)
  * Python in a toolchain?
   * Python is just as eligible for a toolchain as FFTW (or ScaLAPACK) is
  * `-no-OFED` toolchain suffix, please!
   * should be dropped, cfr. issue opened by Xavier
  * how to recompile WRF with e.g. `ictcec` (with CUDA support included), without having to rebuild the world
  * PRACE CPE
   * see https://github.com/fgeorgatos/easybuild.experimental/tree/master/users/fgeorgatos/PRACE
   * 80-90% of the work seems done
   * a kind of PRACE variables module is required
    * can be assisted via `modextravars`
  * site customizations: how can they be done without editing every single easyconfig file?
   * e.g. TotalView `LM_LICENSE_MANAGER`, `TVDSVRLAUNCHCMD`, ...
   * can be done via e.g. `eb --try-amend='modextravars={"TVDSVRLAUNCHCMD": "oarsh"}'` ??
   * or by setting `EASYBUILD_TRY_AMEND` environment variable ??
  * GRIDengine: benchmarking grid systems framework
   * paper available on continuous monitoring (w/ grid benchmarking)
  * next EasyBuild hackathon as a part of a PRACE training event for sysadmins?
  * `cdash` discussion
   * microbenchmarks and tests for MPI stacks
   * performance monitoring of different MPI stacks on systems
 * [prof. Promponas/Fotis]
  * write a small abstract on EasyBuild for bioinformatics community
  * large bioinformatics conference in Berlin, July 2013 => Fotis?

## Hackathon notes

 * Alexander
  * NWChem @ JUROPA
   * `libibverbs`, `libibumad` are missing as deps
   * patch `configure` in `src/tools` for `ga-5.1`: `--with-openib='$LDFLAGS -libverbs -libumad'`
 * George T. & Kenneth
  * discussion on mutable toolchains, e.g. `--tcamend={'FFTW': ('FFTWsp', '3.3.3')}`
 * Dina
  * GCC sanity check failed because it was built on a 32-bit system, but build is OK
  * fixed by Kenneth by copying module in the right place, so `gompi can be created and go from there
  * continued work on `Tclcl` (ok), `otcl` (ok), `NS2` (ok), `NAM` (incomplete but progressing)...
 * Mohamed: automake, UNITE + deps, together with George T.
 * George T. + Fotis
  * `goolf` v1.4.10 and v1.5.10 (OpenMPI v1.6.4 and v1.7rc8)
  * FFTW(sp), GROMACS (CUDA)
   * don't use `--enable-avx` in FFTW for GROMACS, because performance may drop ~20%
    * see http://www.gromacs.org/Documentation/Installation_Instructions#3.2.1._Running_in_parallel
  * also looked into `Charm++` (tricky) and `NAMD`
 * Thekla
   * install `goalf` and `ictce` toolchains on Euclid (and CyTera?)
   * `gzip`, `tar`, more homework by Fotis :-) (but thanks for the contribution anyhow!)
 * George F.
  * finished build `Ferret` with `goalf` toolchain, after further patching/retesting
   * builds with `ictce` toolchain, but `ferret` command then segfaults

<p align="center">
<img src="http://hpcugent.github.com/easybuild/files/EasyBuild_hackathon_Cyprus_2013_whiteboard.jpg" alt="picture of whiteboard after 3rd EasyBuild hackathon @ Nicosia, Cyprus"/><br>
picture of whiteboard after 3rd EasyBuild hackathon @ Nicosia, Cyprus
</p>