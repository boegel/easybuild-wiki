(Tuesday Oct 22nd 2013, 9am-5pm)

The first day of the [[4th EasyBuild hackathon]] consisted of presentations, discussions and
initial hands-on experience with EasyBuild for attendees new to the tool.

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
 * **(extra attendee, afternoon only? ask George)**

(note: remote participation was not a success, no conference calls took place)

## Program

(note: started with some delay due to setting up recording equipment in combination with setup for potential remote attendees)

 * [9.40am - 9.50am] **presentation on LinkSCEEM project** _(Jens Wiegand, CyI)_ **([slides](http://hpcugent.github.io/easybuild/files/EasyBuild_hackathon_Cyprus_Oct13_welcome_LinkSCEEM.pdf))**
 * [10am - 11am] **Introduction to UNITE** _(Dr. Bernd Mohr, JSC)_ **([slides](http://hpcugent.github.io/easybuild/files/EasyBuild_hackathon_Cyprus_Oct13_UNITE.pdf))**
 * [11.10am - 11.15am] **round table: briefly introduce yourself**
 * [11.15am - 1pm] **EasyBuild introduction** _(Kenneth Hoste, UGent)_ **([slides](http://hpcugent.github.io/easybuild/files/EasyBuild_introduction_hackathon-Cyprus-Oct13.pdf))** **(recorded presentation: [part 1](http://www.youtube.com/watch?v=bOeNsfLB2t4) - [part 2](http://www.youtube.com/watch?v=e7fyHtO8_qs))**
 * [2pm - 3pm] **EasyBuild status update** _(Kenneth Hoste, UGent)_ **([slides](http://hpcugent.github.io/easybuild/files/EasyBuild_status-update_hackathon-Cyprus-Oct13.pdf))**
 * [3pm - 5.15pm] **hackathon**
 * [5.15pm - 8pm] aftermath: discussions

**All presentations were recorded by Alan, material will be made available when it has been processed.**

## Presentation notes

### LinkSCEEM presentation (Jens Wiegand, CyI)

**([slides](http://users.ugent.be/~kehoste/welcome_LinkSCEEM.pdf))**

 * goal of LinkSCEEM project: establish HPC ecosystem in Eastern Middle-Terranean
  * resources, training, expertise, connectivity, ...
  * with support from NCSA, Jülich, ..
 * several research projects: EEWRC, STRAC, CaSTORC
 * two aspects
  * CyTera: hardware, structurally funded
  * LinkSCEEM
   * users, community, training
   * providing access
   * bring international expertise into the region
 * expectation is to organize more hackathons like this in the future


### UNITE presentation (Dr. Bernd Mohr, JSC)

**([slides](http://users.ugent.be/~kehoste/cyprus-unite-2013.pdf))**

 * see [https://apps.fz-juelich.de/unite](https://apps.fz-juelich.de/unite)
 * bio
  * 25 years of experience in HPC and performance tools
  * involved with TAU from the beginning, also with Vampir, Scalasca, etc.
 * tools depend on MPI library/versions, only _source code_ compatible
  * => can't mix MPI libraries at runtime
  * some tools even depend on compiler, e.g. for/because of instrumentation
 * tool components can be linked together, e.g. Scalasca uses parts of TAU if its available to provide more functionality, etc.
 * UNITE: provide portable common access to parallel performance tools
  * meta-installer for standardized tool installation (op top of existing environment)
  * standardized access to the tools
  * uses modules internally (which follow own naming scheme)
  * UNITE can simply link to existing tool installations instead of reinstalling the tool again
 * tool modules provided by UNITE provide 'getting started' info via "module help"
  * _[KH]_ very close to (a minimal) EasyDoc (vaporware that has been discussed off and on during EB discussathons)
 * requirements are very basic
  * `/bin/sh` and `make`
  * no 'fancy' dependencies like Python <version X>, Ruby, Lua, Tcl, ...
 * `configure` script is a self-written shell script, that behaves like an `autotools` configure script
 * also provides a `Makefile`, next to additional scripts for flexible package configuration
 * auto-detects compiler, MPI, ...
 * about a dozen performance tools are supported
 * various compilers: gnu, ibm, intel, open64, pathscale, pgi, sun
  * => testing nightmare
 * huge amount of MPI libraries supported
  * HP, Intel, ... (15+)
 * UNITE picks one of the available compilers, MPI libs, has some optional deps
 * uses separate log files per step
  * _[KH]_ EasyBuild should do this! (TODO: open issue)
 * problems to deal with
  * packages evolve, things break
   * API changes, new/different install options, ...
  * changes in system/context/compiler
   * e.g., `lib` -> `lib64`
  * keeping module setup consistent is tough
   * requires to check stuff at runtime (not there yet), to validate symlinked tool installations
    * (re)check context, version, ... (tool might have been reinstalled after UNITE install)
   * how to deal with Lmod (open problem)
 * works on basic Linux clustes, not yet on BlueGene, Cray (TODO: ask Bernd what he thinks about getting EB to work on BlueGene, madness?)
  * even though those environments are much more stable
 * current situation @ JSC
  * standardization across clusters/sites is an issue
   * someone takes care of a software installation (on the side), does a little bit of extra effort so others can use it (e.g. module)
  * people are really religious about their site policy
   * even a UNITE module that symlinks to existing installations is an issue
  * tracking dependency modules is available, but optional
   * _[KH]_ not sure anymore what this is about
  * some modules are not available, except when you're in a particular group (i.e., when you know what you're doing)
  * forget about stable HPC systems, "it's Formula 1", things are going to keep evolve/change at a rapid pace
  * _[KH]_ so, EasyBuild is very interesting to JSC
   * currently no consistency, high 'bus' factors
  * not using system provided tools can be a BIG issue
   * e.g., compiler, (MPI) libraries
    * e.g., because it's tied to SLURM resource manager, network drivers
    * or because vendor is being paid to maintain the tools on the system, so they should really be used as is
   * this is a form of site-specific customizations

### EasyBuild introduction (Kenneth Hoste, UGent)

**([slides](http://users.ugent.be/~kehoste/EasyBuild_introduction_hackathon-Cyprus-Oct13.pdf))**
**(recorded presentation: [part 1](http://www.youtube.com/watch?v=bOeNsfLB2t4) - [part 2](http://www.youtube.com/watch?v=e7fyHtO8_qs))**

 * build dependencies shouldn't be version fixed, should be allowed to float around? (TODO: open issue)
 * is there a way of cleaning up old modules without breaking stuff
  * `eb --remove`
  * e.g., on your laptop this would be very useful
 * why set `$MODULEPATH` hard, instead of using `module use`?
  * `_[KH]_` this might resolve the `MODULEPATH_xyz` stuff being inconsistent with Tcl modules on JUROPA(3) with the DEISA modulecmd.tcl
 * support for `eb --continue`
  * continue after a build breaks, either manually or make EB continue
  * even without specifying it explicitly, just continue based on what it finds
   * e.g., by keeping track of the last completed step in a dedicated file `last_step` (cfr. GCC build, `stage_current`, etc.)
  * interesting for people new to EB
 * EasyBuild configuration should allow specifying a default compiler toolchain to use
 * adjusting easyconfig files (e.g., a bug fix) currently requires tweaking lots of file
  * will be resolved with easyconfig format 2.0
 * _[GT]_ have users provide bash scripts to build software, use that to implement easyblock
  * making people new to EasyBuild who are not very familiar with Python produce an easyblock is quite difficult
 * support for downloading from repository instead of hard URL, e.g. svn, git (see [framework#112](https://github.com/hpcugent/easybuild-framework/issues/112))


### EasyBuild status update (Kenneth Hoste, UGent)

**([slides](http://users.ugent.be/~kehoste/EasyBuild_status-update_hackathon-Cyprus-Oct13.pdf))**
**([recorded presentation](http://www.youtube.com/watch?v=A140WvbqaNw))**

 * new easyconfig format
  * will (significantly) complicate testing
  * keeping support for previous version is very important
   * keep ability to reproduce previous builds
   * consume old format, produce easyconfig in new format (archive, installdir)
 * wiki page on `Contributing back` should have a separate section on testing
  * e.g. unit tests, etc.
 * bootstrap procedure should be mentioned/outlined on `Getting started` page
 * upgrading EasyBuild: how?
  * add support for `--upgrade`?
  * since v1.8.2, `eb EB.eb` will work to produce a new EasyBuild module
 * support for optional dependencies would be nice
  * 3 levels for dependencies: must (only currently supported flavor), should (use it when it's there), may (only use when requested explicitly)
 * _[BM]_: why are `bzip2`, `bash`, `binutils` supported and listed as dependencies?
  * primary reason is fixing versions of dependencies for reproducibility
  * support way of specifying minimal supported version for deps?
 * `"stow"` modules
  * collapse a set of modules into a single symlinked mess
  * or, install a bunch of software packages under a single install path
  * fewer modules, shorter `$PATH` and co, ...
 * framework unit tests
  * some test files (e.g. `toy-0.0.tgz`) not installed (see [#739](https://github.com/hpcugent/easybuild-framework/pull/739) for fix)

## Hackathon notes

 * discussion on new easyconfig format that is being worked on
 * support for Tcl environment modules added together (with _BM_)
 * look into `modules.py` unit tests hard overriding `$MODULEPATH`, causing problems with `purge` if modules are loaded when tests are run (with `_AO`)
 * update documentation of contributing back (with _XB_)
 * discussion on current organization of repositories
  * move `generic` easyblocks to `framework` repo
  * merge `easyblocks` and `easyconfigs` repositories together
   * easier to keep in sync when pull requests are opened, commit IDs remain in sync/consistent
   * less confusing for new contributors, lowers threshold for making PRs for new software
  * or, alternative option (_XB_):
   * merge `framework` and `easyblocks`, since both are code
   * keep `easyconfigs` separate, since they're basically input files