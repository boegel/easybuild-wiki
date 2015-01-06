(back to [[Conference calls]])

Notes on the 23rd EasyBuild conference call, Tuesday January 6th 2015 (3.00pm - 3.30pm CET)

#### Attendees

Alphabetical list of attendees (4):

* Eric Gregory (JSC, Germany)
* Kenneth Hoste (HPC-UGent, Belgium)
* Alan O'Cais (JSC, Germany)
* Robert Schmidt (OHRI, Canada)


#### Agenda

   * outlook to EasyBuild v2.0 (Kenneth)
   * overview of major upcoming features (Kenneth)


#### Notes

##### Outlook to EasyBuild v2.0

* mainly dropping support for deprecated behaviour (and bumping the version)
   * see http://easybuild.readthedocs.org/en/latest/Deprecated-functionality.html
   * `eb --fix-deprecated-stuff` cmdline option?
* no new features in EasyBuild framework planned, probably only in EB v2.1 and later
* hopefully getting a significant amount of easyblock/easyconfig PRs merged in

##### Overview of major upcoming features

* only generating module files (skipping actual build/installation), with sanity check optionally retained
     * https://github.com/hpcugent/easybuild-framework/pull/1018
* support for resolving dependencies via subtoolchains (+ dummy) [Markus/Alan/Eric @ Basel hackathon?]
     * https://github.com/hpcugent/easybuild-framework/issues/741
* support for generating module files in Lua syntax
     * https://github.com/hpcugent/easybuild-framework/pull/1060
* support for module families (Tcl & Lua, Lmod-only)
* support for module properties (Tcl & Lua, Lmod-only)
     * https://github.com/hpcugent/easybuild-framework/issues/981
* support for installing dependencies with minimal required subtoolchain
* easyconfig format v2 (+ enhanced eb cmdline)
* specifying module names to install on EB command line
* support for non-strict version specifications for dependencies
     * https://github.com/hpcugent/easybuild-framework/issues/746
* support for --avail-toolchainopts
* support for --dump-toolchain-env
     * https://github.com/hpcugent/easybuild-framework/pull/1091
* support for --pr-review
     * https://github.com/hpcugent/easybuild-framework/pull/1005
* better support for --job [Ricardo @ Basel hackathon?]
     * https://github.com/hpcugent/easybuild-framework/pull/1008
* support for downloading from SVN/git repository [Jens @ Basel hackathon?]
     * https://github.com/hpcugent/easybuild-framework/pull/1082
* support for easily overriding easyconfig parameters via cmdline/environment
     * https://github.com/hpcugent/easybuild-framework/pull/712
* support for generating packages (via FPM?)
     * https://github.com/hpcugent/easybuild-framework/issues/200
* support for uninstalling software (requires reverse dependency tracking)
     * https://github.com/hpcugent/easybuild-framework/issues/590
* support for rpath
     * https://github.com/hpcugent/easybuild-framework/issues/651
* toolchain-neutral installations
     * https://github.com/hpcugent/easybuild-framework/issues/570
* fix devel modules, create them after each build step
     * https://github.com/hpcugent/easybuild-framework/issues/109

notes:
* overview of features we have in mind for the coming weeks/months
   * no guarantees w.r.t. when they will be worked on!
* of particular interest to HPC-UGent:
   * support for only (re)generating module file, potentially under a different module naming scheme and for existing installations
   * support for module files in Lua syntax and module families/properties
* of particular interest to JSC:
   * support for resolving dependencies via subtoolchains & installing dependencies with minimal required toolchain

##### Other topics

 * issue with binutils build with OS-provided zlib and modules loaded that have another zlib as a dependency (Eric)
    * binutils tools issue a warning due to zlib version mismatch
    * two possible solutions:
      * statically link (OS-provided) zlib into binutils
      * avoid installing a `zlib` module with `eb` using `--filter-deps=zlib` configure option
 * (Perl) script(s) to determine reverse dependencies for EB-installed software (Eric), used for:
   * picking versions of common dependencies (e.g. zlib, Python, ...) and avoid too many different versions of common dependencies
   * determining impact of removing a particular module (that may be used as a dependency for others)
   * determining which toolchain module to use based on what `module spider` specifies
      * Lmod is unaware of toolchains, and thus specifies individual toolchain components to load rather than a single toolchain module