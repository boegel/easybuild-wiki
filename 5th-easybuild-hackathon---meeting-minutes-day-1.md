[[5th easybuild hackathon]] - [notes day 2](https://github.com/hpcugent/easybuild/wiki/5th-easybuild-hackathon---meeting-minutes-day-2) - [notes day 3](https://github.com/hpcugent/easybuild/wiki/5th-easybuild-hackathon---meeting-minutes-day-3)

## presentation: Introduction to EasyBuild

* [slides (PDF)](http://users.ugent.be/~kehoste/EasyBuild_introduction_hackathon-JSC-Feb14.pdf)

#### questions
 * using system-provided tools (compiler, MPI, …)
  * hard requirement for e.g. IBM XL compiler, also for Fujitsu, Hitachi, …
  * can be done via a `SystemTools` easyblock? (not there yet today)
  * implementing compiler support in framework is still required, see `easybuild/toolchains/compiler`
 * 3 (actually 4?) different types of users:
  * people who simple use 'eb' with existing easyconfig files
  * people who write their own easyconfig files for new software
  * people who implement easy block
  * (people who develop framework)

## hackathon: getting started

* need to fix bootstrap script to work with Tcl environment modules and Lmod !!!
 * cfr. https://github.com/hpcugent/easybuild-framework/issues/765
 * **update**: PR available, see https://github.com/hpcugent/easybuild-framework/pull/869

* Pavel: 'fooling' EasyBuild by providing modules for system-provided tools
 * e.g. build Cube with all reps provided by OS
 * get toolchain module exactly right (deps, conflict line, …), `$EBROOTX`, …

* Peter: SSL dep for Python => switch to using OpenSSL sep
 * build Score-P + whole software stack using --robot

* Markus: making EasyBuild generate hierarchical modules
 * basically just playing around with module name space
 * `xx/GCC/4.7`, prepends `$MODULEPATH` with `yy/4.7`
 * `yy/4.7/OpenMPI/1.6.5`, prepends `$MODULEPATH` with `zz/1.6.5`
 * `zz/1.6.5/gzip/1.4`, exposed to user as `gzip/1.4` :-)
 * prepend `$MODULEPATH` via `modextrapaths`, define `$MODPREFIX` for easy access in module naming scheme?
 * get this documented?
 * support for hierarchical modules in modulecmd.tcl can be provided via self-defined Tcl functions; all modules mustsource these
 * **update**: see https://github.com/hpcugent/easybuild-framework/issues/862

* Markus: conflict between toolchain modules (also MPI, maybe compiler/BLAS/LAPACK libs?)
 * can be done via setting env var and writing own conflict detection function (done now already in Tcl)
 * see https://github.com/hpcugent/easybuild-framework/issues/860

* Markus: global way of setting opt arch toolchain option to False?
 * a general config option should be available to tweak toolchain options

* Alexandre: building stat + deps (libddwarf)

* Jens: implement `--track-user` (in log, module file, build stats, …)
 * see https://github.com/hpcugent/easybuild-framework/issues/856

* download of MPFR is broken, sources got moved?