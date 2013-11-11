(Thursday Oct 24th 2013, 9am-6pm)

The third day of the [[4th EasyBuild hackathon]] consisted of attendees working with and on EasyBuild,
trying to build software packages of interest, or adding support for them, and tackling open issues they care about.
Pull requests were openend for the work that was finished during the hackathon.

These notes were taken by Kenneth, suggestions for additions and improvements are very welcome.

## Attendees

 * Yossi Baruch (Isragrid, Israel)
 * Xavier Besseron (University of Luxembourg)
 * Marios Constantinou (University of Cyprus)
 * Stelios Erotokritou (The Cyprus Institute)
 * Fotis Georgatos (University of Luxembourg, HPC sysadmin and active contributor)
 * Kenneth Hoste (HPC-UGent, EasyBuild developer and release manager)
 * Thekla Loizou (Cyprus Institute, local organization)
 * Dina Mahmoud Ibrahim (Cairo University)
 * Dr. Bernd Mohr (Jülich Supercomputing Centre, UNITE)
 * Alan O'Cais (Jülich Supercomputing Centre)
 * Andreas Panteli (The Cyprus Institute)
 * George Tsouloupas (The Cyprus Institute)

(note: remote participation was not a success, no conference calls took place)

## Program

(note: started with some delay due to setting up recording equipment in combination with setup for potential remote attendees)

 * [9am - 6pm] **hackathon** (extended due to issues with the bus in the morning)
 * [6pm - 9pm] aftermath: discussions

## Hackathon notes

 * _Bernd_
  * default output should clearly split output for different builds
   * e.g. include a `--------...-------` line in between
  * working on `Tau`, which uses `-prefix` (instead of `--prefix`), and requires multiple configure/build/install cycles
   * very easy with EasyBuild (after support for tweaking `--prefix` was added to `ConfigureMake` easyblock, see [easyblocks#287](https://github.com/hpcugent/easybuild-easyblocks/pull/287))
   * this is quite convincing since a somewhat complex/non-standard build is so easy to get working
  * `Vampir` is a single-command binary installation, but `Binary` easyblock doesn't allow this easily?
   * `Binary` easyblock should accept an easyconfig parameter like `install_cmd` (TODO: fix this)
   * `Paraver` software package requires list `preconfigopts` to be supported (is it?)
 * _Xavier_
  * `toolchains/mpich.py` and `toolchains/mpich2.py` are almost identical, lots of code duplication! (TODO: open issue)
  * we need an easyblock for software 'bundles' (e.g. `Bundle`) (TODO: fix this)
   * currently, `Toolchain` is used for that, but a toolchain is more than just a collection of dependencies
    * especially when we start setting environment variables for compiler toolchain in the module
  * bug in `HDF5` easyblock when trying to build a serial version (see [easyblocks#286](https://github.com/hpcugent/easybuild-easyblocks/pull/286))
  * discuss setup for making only a part of the modules available to users, via a separate `$MODULEPATH` and symlinking the modules
  * recursive use of `--try-toolchain` isn't working yet, would be really useful if this gets fixed (see [framework#450](https://github.com/hpcugent/easybuild-framework/issues/450))
  * enable building of shared libraries for `MPICH` (see [easyconfigs#482](https://github.com/hpcugent/easybuild-easyconfigs/pull/482))
  * facilitate contribuing to EasyBuild wiki by making a dedicated repository for it (TODO: fix this, document it, announce on mailing list)
   * simply define an additional remote, keeping things in sync should be fairly easy
    * `git pull origin master` for edits directly on wiki
    * `git pull github_hpcugent master` for edits via PRs
    * merge conflicts and push back to `origin` to update wiki pages
  * `iomkl` toolchain module loads both `OpenMPI` (element of toolchain) and `impi` (dependency for `imkl`)? (TODO: open issue)
 * _Kenneth_
  * enhance `ConfigureMake` easyblock such that `--prefix` can be overruled
  * planning for future EasyBuild conf. calls
   * fixed time slot, max. 30m, bi-weekly
    * tick-tock between CET timeslot and US West Coast timeslot (late evening CET)
   * for now, fixed to every 1st/3rd **Tuesday** of the month, **3pm CET**
   * TODO: 
    * set up Google calendar
    * create a wiki page per conf call
    * send reminder day before via EasyBuild mailing list
 * _Andreas_
  * still looking into `UNITE`/`ExtraE` issues with `-fPIC`
   * seems like rebuilding both `binutils` and `GCC` with `-fPIC` would be required
   * should try and see whether removing `-fPIC` from `ExtraE` flags helps
 * _Marios_
  * AMBER: working on a (hackish) easyconfig file
   * also requires an easyblock to resolve things properly in the end
 * _Thekla_
  * looking into `ncdf` and `ncdf4` R packages
   * as a separate module, since they have `netCDF` as dependency
   * ran into a `zlib` version clash between `R` module and `netCDF`
    * resolved by creating new easyconfig for `R/3.0.2`, and using same `zlib` as for `netCDF`
  * add easyconfig for `Silo` and `ParFlow` ([easyconfigs#483](https://github.com/hpcugent/easybuild-easyconfigs/pull/483))
  * add easyconfig for `CDO` ([easyconfigs#484](https://github.com/hpcugent/easybuild-easyconfigs/pull/484))
 * _Fotis_
  * `goolfc` update (see [easyconfigs#477](https://github.com/hpcugent/easybuild-easyconfigs/pull/477))
   * new GCC, OpenMPI and other bits; also employed most recent CUDA contributed by @ajdecon; in short, latest and greatest
   * `-malign-double` should be enabled by default, which is required by CUDA for getting good performance and/or ABI compatibility (see [framework#627](https://github.com/hpcugent/easybuild-framework/issues/627))
  * help people out with questions/problems
 * _Dina_
  * try and get VMD plugins installation working, as a first step towards VMD support
  * results pushed to [`easybuild.experimental`](https://github.com/fgeorgatos/easybuild.experimental/tree/master/users/dina)
 * _Alan_
  * manually create modules for system Intel compiler, MPI libraries, and Intel MKL
   * should set `$EBROOT...` and `$EBVERSION...`
   * version for `icc`, `ifort` and `imkl` modules is important to get exactly right
   * version of `MPICH2` module is less important, should include versionsuffix that indicates it's a custom version (`ParaStation`)
   * need separate modules that each set the correct value for `conflict` (same as module name)
    * EasyBuild relies on this when checking toolchain definition against elements of toolchain module
   * create `impmkl` easyconfig file to obtain toolchain module
   * add `impmkl` toolchain definition to EasyBuild (_Kenneth`_, see [framework#736](https://github.com/hpcugent/easybuild-framework/pull/736))
 * _Yossi_
  * add easyconfig for new version of `scipy` ([easyconfigs#480](https://github.com/hpcugent/easybuild-easyconfigs/pull/480))
  * add easyconfig for `IPython` ([easyconfigs#485](https://github.com/hpcugent/easybuild-easyconfigs/pull/485))
  

## Discussion notes

(with _Xavier_, _Fotis_, _Kenneth_)

 * jail tool is bottom-up
  * _FG_: if you whitelist something, you may start missing stuff again (ie. not reporting osdependencies properly)
   * _KH_: only if you resolve it by allowing OS deps?
   * _FG_: it is probably very wise to adhere to the rule "osdependencies reported via deps, are redundant and not needed"
  * top-down: via `ldd`
   * but, then stuff like static libs, required header files, etc. are missed
  * _XB_: `makeinstall` tool to easily create packages, but doesn't take into account deps, etc.
  * _FG_: `checkinstall` is probably what we want to refer here; ref. http://asic-linux.com.mx/~izto/checkinstall/
 * _FG_: getting environment modules 3.2.10 into RHEL6
  * required to reel in grid people; _FG_ has already been bugging Gergely Sipos since spring 2013 about EB.
  * EasyBuild is a (very) consistent way to roll out software, even cross-distro, and pretty possible cross-OS
 * technical reason for using only `top` (ictce/goolf/...) toolchains, and just rebuilding tools like `CMake` with those
  * recursive `--try-toolchain` will not work with other approaches to only obtain a minimal set of modules that easily allow switching MPI stacks
   * rebuilding software stack with another toolchain would require lots of manual work
 * [off-topic] discuss Valentin's (Uni.lu) work on using GAs to autotune configure options for best performance
  * using EasyBuild to rebuild software packages with different setup
  * mainly for `GROMACS`, `QuantumESPRESSO` (and considering `RaxML` for its fancy MPI with pthreads hybrid model)
  * should be *really* strict about testing and validating correctness of resulting binaries
  * also consider using modules to predict outcomes and let that steer the GA, to significantly speed up search process
  * _FG_: `RaxML` is a special case; it is significantly tuned to specific processor architectures (eg. x86 L1/2/3 caches)
   * which makes it extremely sensitive to the value of particular config options and run-time MPI stack tuning
   * can machine learning models catch/learn something like that? what can we extract and automate out of this?
