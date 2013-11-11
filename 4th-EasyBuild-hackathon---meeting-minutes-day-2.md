(Wednesday Oct 23rd 2013, 9am-5pm)

The second day of the [[4th EasyBuild hackathon]] consisted of attendees working with and on EasyBuild,
trying to build software packages of interest, or adding support for them, and tackling open issues they care about.

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

 * [9am - 4.50pm] **hackathon**
 * [4.50pm - 5.20pm] **round-table: who is working on what, how is it going**
 * [5.20pm - 7.50pm] aftermath: discussions

## Hackathon notes

 * _Fotis_
  * help people out with questions/problems
 * _Bernd_
  * get EasyBuild working with Tcl environment modules, get builds started with `gompi` toolchain (on laptop)
  * issues with getting GCC (and OpenMPI) build to work
   * sanity check fails on `OpenSUSE` (see [easyblocks#283](https://github.com/hpcugent/easybuild-easyblocks/issues/283))
   * laptop shut down automagically due to heat issues...
  * started looking into building performance tools with EasyBuild
   * `opari2` works
   * goal: Scalasca with dependencies
  * also interested in getting compiler toolchain with system compiler/MPI to work (cfr. work by Alan)
  * EasyBuild looks very good, should work for UNITE after spending some time with it
   * build modules for performance tools in UNITE, then just symlink to those installations
 * _Kenneth_
  * implement script to easily (re)install EasyBuild `develop` branches
  * answer various big/small questions popping up
 * _Alan_
  * get EasyBuild working on JUROPA3
   * various issues with DEISA version of modulecmd.tcl (outdated compared to latest Tcl environment modules)
    * warnings about `$MODULEPATH_modshare` not being up to date
     * enforced by parameter `g_force` being enabled in `modulecmd.tcl`
     * can not be changed by user if system `modulecmd.tcl` is used
     * can be fixed by using `module use <path` instead of hard setting `$MODULEPATH` (?)
    * switched to using latest `modulecmd.tcl` version available online
  * toy tests are broken due to hardcoded `$MODULEPATH` in `modules.py`
  * issues with `modulecmd.tcl` being called with `subprocess.Popen` under a `bash` shell (see [framework#733](https://github.com/hpcugent/easybuild-framework/issues/733))
   * caused by fact that (old) DEISA version of `modulecmd.tcl` doesn't have a proper hashbang
  * would like to focus next on composing modules to wrap around system compilers/MPI
 * _Dina_
  * look into installing `VMD`
   * (minimal?) dependencies: `FLTK`, `Mesa`, `Tcl`, `Tk`, and `netCDF`
   * `VMD` installation is a mess, will focus on getting first part for `plugins` working as a separate installation
  * add support for FTLK
  * results pushed to [`easybuild.experimental`](https://github.com/fgeorgatos/easybuild.experimental/tree/master/users/dina)
  * bugs uncovered
   * `source_urls` for Mesa do not include path for old releases (see [easyconfigs#478](https://github.com/hpcugent/easybuild-easyconfigs/issues/478))
   * `--dry-run` doesn't take available modules into account, requires easyconfigs files to be available for **all** dependencies (see [framework#731](https://github.com/hpcugent/easybuild-framework/issues/731))
 * _Yossi_
  * building WRF with `goolf` on a VM (see [easyconfigs#479](https://github.com/hpcugent/easybuild-easyconfigs/pull/479))
   * builds requires more than 1GB of memory to avoid heavy swapping
   * minimal requirements should be specified for a system to use EasyBuild on
   * making build stats available publicly would help people in estimating system requirements
  * start looking into using EasyBuild on EGI (European Grid Initiative) grid; (FG: let's make sure RHEL6 gets env-modules >=3.2.10) 
    ref: https://bugzilla.redhat.com/show_bug.cgi?id=976369 # Vote this, otherwise EGI & EasyBuild marriage will be painful
 * _Thekla_
  * `arpack-ng` is working (see [easyconfigs#481](https://github.com/hpcugent/easybuild-easyconfigs/pull/481))
  * `ParFlow` with `Silo` dependency works
 * _Xavier_
  * build `HDF5` for playing around with MPI stacks, with a minimal set of modules by carefully selecting toolchains for dependencies
  * issue for OS dependencies on Debian fixed (see [framework#732](https://github.com/hpcugent/easybuild-framework/pull/732))
 * _Marios_
  * working on building AMBER 9 with ifort, manual installation
   * problem: `configure` has no prefix option, and EasyBuild hardcodes it
 * _Andreas_
  * install `UNITE` with EasyBuild
   * `ExtraE` requires `binutils` but then with `-fPIC` arise
 * _George_
  * looking into installation of `PGI`
   * very interactive, no automation options, download slow
   * also installs CUDA, MPICH, ...
   * unclear if PGI also works with CUDA, MPICH
   * CUDA is probably required for C/C++ CUDA compiler

 * discussion during dinner
  * _Bernd_: biggest issue now is logging and error messages being produced
   * when stuff goes wrong, people new to the tool are lost
   * almost impossible for newcomers to find their way in log files
    * lack of documentation, guidelines, ...
  * (os)dependencies are often missing in easyconfig files
   * [`HashDist` jail tool](https://github.com/hashdist/hdist-jail) should resolve this
    * _KH_ tried it, couldn't get it to work as expected
   * _GT_: try a more naive approach after the software is built using `ldd` and checking what is being linked in from the OS
    * _KH_: won't include static libraries, header files required during configure/build, etc.
   * _FG_: there is no _perfect_ or _naive_ approach here! `ldd` does top-down check, while `hashdist` and `builds-on-minimal-OS` do bottom-up.
     Both are useful and should be examined together, as build confinement options.

## Discussion notes

(with _Xavier_, _Fotis_, _George_, _Kenneth_

 * development setup should be documented
  * simply involves setting `$PATH` and `$PYTHONPATH`
  * be careful with `.pyc` files for files that are in one branch but not in another
 * full fool-proof EasyBuild installation procedure may help newcomers
  * provide a box on wiki page that can be pasted that sets up everything
  * see whiteboard picture:
   * set `$EASYBUILD_INSTALLPATH`, with a `CHANGEME` comment
   * bootstrap to `$EASYBUILD_INSTALLPATH`
   * set `$MODULEPATH` and run `module load EasyBuild`
   * run `eb --version`
   * indicate stuff to add to `.bashrc` (but don't do it ourselves!)
 * questions/concerns w.r.t. mixing software builds done by various EasyBuild versions
  * bugs fixed in new releases may cause builds to be different, how to know which builds are affected? (FG: not possible)
  * one solution may be to include the EasyBuild version in the `installpath`
   * but this still makes mixing possible with multi-dir `$MODULEPATH`
  * only real solution is to rebuild the whole software stack for a new release of EasyBuild
   * currently done at CyI and Uni.lu (_GT_, _FG_) (FG: this guarantees maximum reproducibility & predictable rollback)
 * which policy should be used w.r.t. making modules available to users?
  * providing all modules is an issue, because there's too much (_XB_)
  * only provide current and last version, maybe previous version too
   * this yields concerns w.r.t. reproducibility
  * only solution seems to be solving this with a modules tool that allows setting defaults (e.g. toolchain), hiding stuff (only `avail`, not `load`) while still making them easily accessible when needed, etc.
  * brings back discussion on a Python modules tool
   * is it worth the effort?
   * can we make the shift transparent by providing wrappers that mimic the quirky behavior of other module tools?
  * C environment modules also support caching (for `avail`) (_XB_)
  * why is running `module avail` faster the 2nd time (_FG_)
   * can't just be the file system cache, something else must be going on top of that (TCL could be a contributor)
 * support for custom module naming schemes ([framework#687](https://github.com/hpcugent/easybuild-framework/issues/687))
  * should include `moduleclass` too, to provide more flexibility
  * **always** use EasyBuild flat module naming scheme
   * as a fallback in case the custom module naming scheme fails at some point
   * 'on the side' and hidden, e.g. in `$EASYBUILD_INSTALLPATH/modules/.easybuild/all`
   * or, simply allow for multiple views available in parallel: (_FG_)
    * $EASYBUILD_INSTALLPATH/modules/all                    # default flat, some systems can ONLY handle this one
    * $EASYBUILD_INSTALLPATH/modules/bycategory/{bio}...    # the existing categories should be under a collective dir
    * $EASYBUILD_INSTALLPATH/modules/bytoolchain/{goolf}... # really useful to reduce module avail output
    * $EASYBUILD_INSTALLPATH/modules/custom                 # combinations of the above or other custom schemes
