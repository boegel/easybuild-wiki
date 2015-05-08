![EasyBuild hackathon @ Basel](http://users.ugent.be/~kehoste/EasyBuild-hackathon-Espoo-group-picture_20150505-cropped.jpg)

### Basic Information

* **date**: Monday May 4th - Tuesday May 5th 2015
* **venue**: Multifunction room Debatti, 1st floor at Life Science Center, Keilaranta 14, CSC, Espoo, Finland (see https://www.csc.fi/contact-info)
* in conjunction with the Nordic e-Infrastructure Conference (NeIC) see http://neic2015.nordforsk.org/
* **organisers**: Kenneth Hoste (HPC-UGent), Olli-Pekka Lehto (CSC.fi)

### Attendees

For practical reasons, the number of participants is limited.

Please contact Kenneth Hoste (kenneth.hoste@ugent.be) or Olli-Pekka Lehto (olli-pekka.lehto@csc.fi) to register.

list of confirmed attendees:

* **Luis Alves** (CSC, Finland)
* **Jani Heikkinen** (CSC, Finland)
* **Petar Forai** (IMP/IMBA, Austria; EasyBuild user and contributor)
* **Johan Guldmyr** (CSC, Finland)
* **Kenneth Hoste** (HPC-UGent, Belgium; EasyBuild developer and release manager)
* **Dan Jonsson Johan** (University of Tromsø, Norway)
* **Olli-Pekka Lehto** (CSC, Finland) **excused**
* **Marco Passerini** (CSC, Finland)
* **Jarmo Pirhonen** (CSC, Finland)
* **Ari-Matti Saren** (CSC, Finland)
* **Atte Sillanpää** (CSC, Finland)

## Topics

Attendees are free to work on what they want, but a couple of topics of interest are typically attached to each hackathon. The intention is to have discussions on these topics, and that some attendees work on these issues:

* Introduction to EasyBuild
* EasyBuild on Cray

## Agenda

_(initial agenda, subject to change)_

The hackathon slots are intended to work on what attendees are most interested in, with support from the experts.

### Monday May 4th 2015

* 9am - 10am: setup, coffee, introductions 
* 10am - 12pm: **Introduction to EasyBuild (and Lmod)** (Kenneth Hoste) ([slides](
http://users.ugent.be/~kehoste/EasyBuild-intro-Espoo_20150504.pdf))
  * incl. demos, Q&A
* _12pm - 1pm: lunch_
* 1pm - 2pm: **EasyBuild on Cray: current status & open problems** (Petar Forai) ([slides](http://hpcugent.github.io/easybuild/files/EB-on-CLE-status-May-2015.pdf))
* 2pm - 3pm: Getting started with EasyBuild and Lmod: installation and configuration, basic usage (hands-on)
* 3pm - 5pm: hackathon

### Tuesday May 5th 2015

* 9am - 12pm: hackathon (continued)
* _12pm - 1pm: lunch_
* 1pm - 5pm: hackathon (continued)

## Notes

### Monday

* Luis: use EasyBuild to install software in CVMFS
  * via Stratum server to have write access to filesystem

* build and install without having modules at all?
  * EasyBuild relies heavily on modules, for resolving dependencies and checking what's already there
  * use --installpath-modules to locate modules somewhere else, without exposing them to users
  * both, then you're on your own w.r.t. providing runtime libraries, etc. (which may not be a big problem when linking statically or using rpath)

* (Jarmo) 'module purge' should never be used on a Cray, default set of loaded modules is too fragile
  * use 'module swap' instead (sidenote: also in generated modules?)
  * this issue came up when running into problems when (re)loading the `slurm` module after loading `PrgEnv` (after using `purge`)

* problem with system-installed Lmod (which is too old) still being picked on Taito, via `$LMOD_CMD` which gets redefined via `/etc/profile.d`
  * will be fixed in EasyBuild v2.1.1, see https://github.com/hpcugent/easybuild-framework/pull/1275

* support for `eb --show-current-config` would be really useful, since it's not always clear which configuration has been done on the various levels, and how these interact (e.g. --prefix with $EASYBUILD_INSTALLPATH, etc, etc)

* (Jarmo) support for directly specifying compiler flags (i.e. $CFLAGS value, etc); useful to have full control if needed (e.g., when benchmarking)

* using system tcsh avoids hanging WRF build on Sisu => don't include EasyBuild-built tcsh as a build dep?

* how to avoid stepping on each others toes when having multiple instances running?
  * problem is not really unique to EasyBuild
  * support for using --job with GC3Pie may help?

* (Kenneth) source_urls for netCDF are broken, old sources have been moved to /old

* (Johan) --search should only take basename into account when matching with provided pattern, not whole path

### Tuesday

* (Atte) parallel build can be reenabled for recent CP2K versions (works fine for 2.6.1)
   * drop `parallel = 1` in recent CP2K easyconfig files

* (Atte) GROMACS requires that a script is sourced before it is used, how can that be handled in module file?
   * similar for OpenFOAM
   * module file can not source a script (why not? Kenneth will ask Robert)
   * msgonload to remind user to source the script
   * use env2 tool (see Lmod's contrib dir) to generate module file for script to-be-sourced
        * supports both Tcl and Lua module files
        * just evaluates, doesn't retain login in script, so far from perfect

* performance testing of CP2K
    * benchmarks included in CP2K sources (see tests/QS/benchmark*)
    * benchmarks rely on fact that ../../../data is around
    * simple H2O benchmarks, different sizes
    * benchmark_HFX: test for psmp, 10x faster with hybrid functionals
    * how to build a static build of CP2K (doesn't work in 1-2-3 on Cray)

* (Jarmo) support for using external modules as toolchain in EB
    * e.g. existing `intel` modules on Taito
    * provide compilers + MPI + MKL + IPP + TBB
    * FFT is enabled in MKL for some of them (automagically available in recent versions, according to Ake?)
    * should be possible via (very simple) Python module for custom toolchain, e.g. intel-taito, cfr. approach for Cray modules
    * support for --custom-toolchains configure option should be added (next to --custom-easyblocks, --custom-module-naming-schemes)
    * see https://github.com/hpcugent/easybuild-framework/issues/1010

* missing documentation on use of `--try` @ easybuild.readthedocs.org

* (Jarmo) modules for Intel tools do not set $MIC_* environment variables yet
    * Taito provides a bunch of nodes with MIC cards

* `--try-amend=runtest=False` doesn't work as expected: `False` vs `'False'`
    * can only be resolved by properly handling types for easyconfig parameters

* `--try-software-version --try-toolchain --dry-run` => recursion is disabled entirely when `--try-software-version` is
  used, which is wrong; only changing version should not be applied recursively, rest should!

* missing cmdline option: `eb --list-software`
