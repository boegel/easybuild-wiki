![EasyBuild hackathon @ Basel](http://users.ugent.be/~kehoste/EasyBuild-hackathon-Basel-group-picture_20150211-resized.jpg)

**All slides are available at [[8th EasyBuild hackathon]].**

## EasyBuild questions

* reusing existing modules for toolchain used by EasyBuild?
    * if the module is available, EasyBuild will assume dependency is available
    * the `setenv EBROOTFOO` and `setenv EBVERSIONFOO` lines should be included for certain things to work
        * e.g., the `get_software_root` function used by some easyblocks
    * in some cases, easyconfig files are still required for dependencies (`--dry-run`, `HierarchicalMNS`, ...)

## EasyBuild ideas/suggestions/feature requests

* request for support for performing build procedure step-by-step (`--stop`, `--continue`)
    * configure/build with user A, install with user B
* request for support for patching sources via `sed` expressions, to avoid that patches break with new releases

## EasyBuild/Lmod to aligning software stacks across (Swiss) sites

* common toolchains `foss` and `intel` are very useful to focus efforts across sites
* discussion w.r.t. aligning Swiss sites
    * suggestion by Vital-IT to use RPM packages as an easy way to distribute software installations and module files
    * separate repository can be set up for easy distribution of the self-built RPMs
    * RPMs can also be installed on non-RPM distros (Debian, Ubuntu, etc.)
    * Vital-IT's current approach is to build RPMs via `.spec` files
    * installation prefix is kept fixed, yet may be different type of filesystem (local, shared) on different systems
    * support in EasyBuild for creating software packages should be looked into, FPM looks promising
        * proof-of-concept by Gianluca and Fotis @ [framework#1170](https://github.com/hpcugent/easybuild-framework/pull/1170)

## EasyBuild hackathon

* nice mix of people new to the tools (EasyBuild, Lmod, XALT) and experienced users

### EasyBuild

* **Adam**: work on improving own easyconfigs, become more familiar with contribution process
 * see Coot @ [easyconfigs#1400](https://github.com/hpcugent/easybuild-easyconfigs/pull/1400), Pmw @ [easyconfigs#1403](https://github.com/hpcugent/easybuild-easyconfigs/pull/1403), PyMOL/GLEW @ [easyconfigs#1405](https://github.com/hpcugent/easybuild-easyconfigs/pull/1405)
* **Alan and Eric**:
 * work on toolchain-based hierarchical module naming scheme
  * see [framework#1168](https://github.com/hpcugent/easybuild-framework/pull/1168)
 * work out how support for resolving dependencies using subtoolchains could be added
  * specify list of subtoolchains in toolchain definition in framework
  * indicate versions for subtoolchains in toolchain easyconfigs
  * also think about equivalence classes for toolchain: `compiler`, `compiler_mpi`, `full` (compiler, MPI, BLAS, LAPACK, FFTW), etc.
 * Eric: demo scripts for keeping module tree clean w.r.t. minimizing different versions of dependency modules
* **Danilo**: first steps with EasyBuild, look into new version of `gmvolf` toolchain
 * see [easyconfigs#1397](https://github.com/hpcugent/easybuild-easyconfigs/pull/1397)
* **Fotis**: get various open easyconfig pull requests ready for merging (and merged)
 * see FSA/PRANK/SMALT/Infernal @[easyconfigs#568](https://github.com/hpcugent/easybuild-easyconfigs/pull/568), rCUDA @ [easyconfigs#720](https://github.com/hpcugent/easybuild-easyconfigs/pull/720), goolf v1.7.20 @ [easyconfigs#1294](https://github.com/hpcugent/easybuild-easyconfigs/pull/1294), Lmod 5.9 @ [easyconfigs#1394](https://github.com/hpcugent/easybuild-easyconfigs/pull/1394)  
* **Gianluca**: work on own easyconfig files @ Novartis
* **Gianluca/Fotis**: look into initial support for letting EasyBuild package software using FPM
    * see [framework#1170](https://github.com/hpcugent/easybuild-framework/pull/1170)
* **Jens**:
 * continue work on adding support for downloading sources from an SVN repository
  * see [framework#1082](https://github.com/hpcugent/easybuild-framework/pull/1082)
 * look into creating RPM packages for EasyBuild, fix related issues
  * see [framework#1166](https://github.com/hpcugent/easybuild-framework/pull/1166)
* **Kenneth**:
 * review work by Petar on support for Lua module files, and help out where needed (see [framework#1060](https://github.com/hpcugent/easybuild-framework/pull/1060)
 * merge contributions made during hackathon that are eligible for merging (reviewed/tested)
 * fix, test, merge bug fix for `--force` not honoring dependencies listed to be installed 'hidden', see [framework#1161](https://github.com/hpcugent/easybuild-framework/pull/1161)
 * stop always updating Lmod user cache, add support for updating Lmod cache at a specific location
        * see [framework#1177](https://github.com/hpcugent/easybuild-framework/pull/1177)
* **Markus**: look into custom module naming scheme based on categories + various small bug fixes
 * fix `path_matches` function + unit tests, see [framework#1163](https://github.com/hpcugent/easybuild-framework/pull/1163)
 * fix `mpi_family` method for non-MPI toolchains [framework#1164](https://github.com/hpcugent/easybuild-framework/pull/1164)
 * clear out `checksums` when `--try-software-version` is used, see [framework#1169](https://github.com/hpcugent/easybuild-framework/pull/1169)
 * suppress creation of module symlinks for `HierarchicalMNS`, see [framework#1173](https://github.com/hpcugent/easybuild-framework/pull/1173)
 * `CategorizedHMNS`, see [framework#1176](https://github.com/hpcugent/easybuild-framework/pull/1176)
 * make Score-P easyblock usable for subtoolchains w/o MPI [easyblocks#552](https://github.com/hpcugent/easybuild-easyblocks/pull/552)
 * source checksum in GCC easyconfigs, see [easyconfigs#1391](https://github.com/hpcugent/easybuild-easyconfigs/pull/1391)
 * consistently use CLooG 0.18.0 for GCC 4.8 series [easyconfigs#1392](https://github.com/hpcugent/easybuild-easyconfigs/pull/1392)
* **Pablo and Martin**: look into new `intel` and `iomkl` toolchains
    * see [easyconfigs#1393](https://github.com/hpcugent/easybuild-easyconfigs/pull/1393) and [easyconfigs#1401](https://github.com/hpcugent/easybuild-easyconfigs/pull/1401)
* **Pablo**: get various open easyconfig PRs ready for merging (and merged)
  * see GATK 3.3 @ [easyconfigs#1155](https://github.com/hpcugent/easybuild-easyconfigs/pull/1155), Java deps in EMBOSS easyconfigs @ [easyconfigs#1167](https://github.com/hpcugent/easybuild-easyconfigs/pull/1167), Picard 1.119 @ [easyblocks#522](https://github.com/hpcugent/easybuild-easyblocks/pull/522) and [easyconfigs#1269](https://github.com/hpcugent/easybuild-easyconfigs/pull/1269), cutadapt @ [easyconfigs#1282](https://github.com/hpcugent/easybuild-easyconfigs/pull/1282)
* **Petar**:
  * continue effort on contribution for adding support for Lua module files, see [framework#1060](https://github.com/hpcugent/easybuild-framework/pull/1060)
  * discuss further steps for supporting EasyBuild on Cray with Tim and Guilherme
* **Riccardo**: continue work on enhancing support for letting EasyBuild submit installations as jobs (via `--job`)
 * see [framework#1008](https://github.com/hpcugent/easybuild-framework/pull/1008)
 * including various suggestions for improvements in the EasyBuild framework
   * see [framework#1171](https://github.com/hpcugent/easybuild-framework/pull/1171), [framework#1174](https://github.com/hpcugent/easybuild-framework/pull/1174), [framework#1179](https://github.com/hpcugent/easybuild-framework/pull/1179)

### Lmod

* Robert: further speed improvements, minor bug fixes, release Lmod v5.9

### XALT

* Robert & Georg: work together on XALT
    * avoid relying on `pstree`, replace with `psutil` Python module so XALT also works on OS X
    * discuss contribution by Georg on adding ORM support

