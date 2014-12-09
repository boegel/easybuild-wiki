(back to [[Conference calls]])

Notes on the 22nd EasyBuild conference call, Wednesday December 9th 2014 (3.00pm - 3.30pm CET)

#### Attendees

Alphabetical list of attendees (9):

* Pablo Escobar (UniBas - SIB, Switzerland)
* Petar Forai (IMP/IMBA, Austria)
* Fotis Georgatos (freelancer)
* Markus Geimer (JSC, Germany)
* Andy Georges (HPC-UGent, Belgium)
* Kenneth Hoste (HPC-UGent, Belgium)
* Alan O'Cais (JSC, Germany)
* Ward Poelmans (UGent, Belgium)
* Jens Timmerman (HPC-UGent, Belgium)


#### Agenda

* update on the upcoming EasyBuild v1.16.0 release (Kenneth)
* thoughts on support for building with minimal required toolchain/dependencies (Alan)
* conflicts for libraries like Szip when you load two separate modules that depend on different version (Alan)
* discuss handling of backlog of open pull requests (Kenneth)
* common toolchains: `intel/2015a` and `foss/2015a` (Kenneth)


#### Notes

##### EasyBuild v1.16.0

 * release planned for Wed Dec 17th 2014
 * a couple of things to tackle:
   * open pull requests by JSC w.r.t. MPICH support and MPICH2 renamed to MPICH
     * cfr. https://github.com/hpcugent/easybuild-framework/pull/1072, https://github.com/hpcugent/easybuild-framework/pull/1073, https://github.com/hpcugent/easybuild-framework/pull/1098, https://github.com/hpcugent/easybuild-framework/pull/1101
   * correctly handling deprecated behavior, making sure `--deprecated` can be used as intended (PR pending)

##### Common toolchains: `intel/2015a` and `foss/2015a`

 * options: https://gist.github.com/boegel/13af5242c01362e38817
 * check Intel tools/GCC compatibility matrix
   * https://software.intel.com/en-us/articles/intel-mpi-library-and-composer-xe-compatibility
   * https://software.intel.com/sites/products/collateral/hpc/compilers/intel_linux_compiler_compatibility_with_gnu_compilers.pdf (pages 17-18)
 * propose toolchain definition & test building existing `intel`/`foss` easyconfigs using `--try-toolchain`

##### Limit number of different available versions for dependencies

 * having different versions of common dependencies available lead to problems
   * examples: zlib, Szip, ...
 * script available at JSC that recommends versions to bump
 * cfr. HPCBIOS "toolchains", in which software packages with common dependencies are bundled together
 * Lmod should simply swap out one version for another rather than report conflicts?

##### Awareness of subtoolchains

 * issues with EasyBuild not being aware of subtoolchain specifications for dependencies
  * problematic with `--try-toolchain-version`: subtoolchains specifications for dependencies are not updated
  * making EasyBuild consider subtoolchains when resolving dependencies should fix this issues
    * not only when trying to find modules for each of the dependencies, but also when installing modules for the unresolved dependencies (e.g. e.g. build CMake with GCC 4.9.2 instead of GCC 4.8.3, rather than building it with goolf v4.x)
  * generic way of specifying toolchain requirements, e.g. `compiler-only`, `compiler-mpi`, etc.?

##### Handling of backlog of open pull requests

 * more reviewers/testers needed (Petar, Pablo?)
 * requires more automation: automatic testing and submitting test reports via Jenkins & dedicated VMs
 * automatic style review, with report being posted back by Jenkins

##### Other topics

 * "two-level" hierarchy based on toolchains (Petar)
  * feedback from Markus & Alan: JSC is also thinking this