(back to [[Conference calls]])

Notes on the 26th EasyBuild conference call, Tuesday March 3rd 2015 (3.00pm - 3.30pm CET)

#### Attendees

Alphabetical list of attendees (7):

* Petar Forai (IMP/IMBA, Austria)
* Andy Georges (HPC-UGent, Belgium)
* Eric Gregory (JSC, Germany)
* Kenneth Hoste (HPC-UGent, Belgium)
* Alan O'Cais (JSC, Germany)
* Ward Poelmans (UGent, Belgium)
* Robert Schmidt (OHRI, Canada)


#### Agenda

   * EasyBuild v2.0 update (Kenneth)
   * early outlook to EasyBuild v2.1
   * make `eb` consider robot search path when looking for easyconfigs specified on the command line
   * dummy vs system toolchain

#### Notes

##### EasyBuild v2.0 update

   * all planned changes in EasyBuild framework have been merged into `develop`
   * a couple of documentation updates must be tackled before EasyBuild v2.0 can be released (major ones are already tackled)
   * see https://github.com/hpcugent/easybuild-framework/issues/1000 for all details
   * full regression test revealed only one major problem w.r.t. `mpi_cmd_for` function affecting several builds (WRF, 

##### Early outlook to EasyBuild v2.1

   * preliminary target is before mid-April
   * definitely before early May, so EasyBuild v2.1 is out for the hackathon in Finland (May 4-5 2015, Espoo)
   * several major new features:
     * support for Lua module files (see https://github.com/hpcugent/easybuild-framework/pull/1060)
     * support for using GC3Pie as a backend for `--job` (see https://github.com/hpcugent/easybuild-framework/pull/1008)
     * support for updating Lmod caches (both user & system) (see https://github.com/hpcugent/easybuild-framework/pull/1177)
     * support for `--review-pr` (see https://github.com/hpcugent/easybuild-framework/pull/1142)
     * support for using EasyBuild on Cray (PRs by Petar are pending)
   * one major change w.r.t. logging errors
     * deprecate use of `log.error(...)` (and `log.exception`)
     * use `raise EasyBuildError(...)` instead
     * see https://github.com/hpcugent/easybuild-framework/pull/1183
     * triggered by work on GC3Pie (https://github.com/hpcugent/easybuild-framework/pull/1008)

##### Consider robot search path when looking for easyconfigs specified on the command line

   * see discussion on EasyBuild mailing list (https://lists.ugent.be/wws/arc/easybuild/2015-02/msg00039.html)
   * no good reason why `eb` wouldn't consider robot search path for easyconfig files specified on cmdline
   * conf call attendees agree it would be a sensible/good change to make that happen
   * would be nice to get that included for EasyBuild v2.0

##### Dummy vs system toolchain

   * `dummy` toolchain was never meant to be used directly, other than for (building and) installing compilers
   * (build) dependencies are *not* loaded when version of `dummy` toolchain is non-empty (e.g. set to `dummy`)
   * EasyBuild framework doesn't set up a proper build environment when a `dummy` toolchain is used
     * e.g. `CC=dummy`, no `$CFLAGS` defined, `toolchainopts` is ignored, etc.
   * a proper system toolchain should be looked into
   * `system-gcc` should be quite easy to do introduce, since good GCC support is already there