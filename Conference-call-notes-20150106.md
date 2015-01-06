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
   * overview of major upcoming features, see https://gist.github.com/boegel/ffc1175183e60cbcda77 (Kenneth)


#### Notes

##### Outlook to EasyBuild v2.0

* mainly dropping support for deprecated behaviour (and bumping the version)
   * see http://easybuild.readthedocs.org/en/latest/Deprecated-functionality.html
   * `eb --fix-deprecated-stuff` cmdline option?
* no new features in EasyBuild framework planned, probably only in EB v2.1 and later
* hopefully getting a significant amount of easyblock/easyconfig PRs merged in

##### Overview of major upcoming features

 * see https://gist.github.com/boegel/ffc1175183e60cbcda77
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