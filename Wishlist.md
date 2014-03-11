Here is a list of features that are desired to have in next versions of EasyBuild:
(importance of them varies according to who you ask ;-)

# USER-oriented features:
* [ ] Add possibility to save the logfile somewhere even when failures happen (/tmp is not great for queue-submitted jobs!)
 * [x] interim solution is to use -ld; OK.
 * [x] ANSWER: "/tmp" was hardcoded before, now OK, issues opened for this; See #84, #695 (ref. $TMPDIR / --tmpdir)
* [ ] Consider support of "formal" language to describe *package info & dependencies*, eg. CUDF syntax
 * http://blog.mancoosi.org
 * http://www.mancoosi.org/papers/cbse11.pdf "MPM: a modular package manager"
 * http://arxiv.org/abs/1109.0456 "Aligning component upgrades"
 * ANSWER: may consider; TBD
* [x] Consider category-oriented module sets (eg. `module load bioinformatics-tools`)
 * [x] may be done outside of EB logic, IFF the meaning is to mangle MODULEPATH? "moduleclass" can help
 * [x] ANSWER: either as said OR, produce an easyconfig with multiple dependencies; TBD
* [x] Allow for custom-definition of easyblock paths (instead of enforcing any "a-z/" dir structure
 * [x] ANSWER: it seems to be OK w. PYTHONPATH? almost, first a-z dir needs to have "reverse" mapping (ref. ".."); TBD
  * KH: the a-z trick can only be used once, other easyblocks repos must use the flat structure (cfr. https://github.com/hpcugent/easybuild/wiki/Setting-up-your-own-easyblocks-repository)
* [x] How to organize a collection of .eb files coming from different sources? (eg. distinct git repos)
 * [x] Seen at http://joeyh.name/code/mr/ : "When updating a git repository, pull from two different upstreams and merge the two together."
 * [x] ie. can EasyBuild --robot dependency resolution support multiple paths? (yes, since v1.10)
 * [x] ANSWER: will support multiple directories; prefer to use many indeed; (v1.10 OK)
* [ ] Consider integrated support for relocatable .rpms (with --prefix), .srpms & RPM's .spec files
 * [ ] in effect, let's create a handler for non "*.eb" suffixes to be managed externally; that would allow
   to create from eg. a .spec file (for RPMs) the equivalent .eb, then parse that stdio->stdout and spit out an easyconfig 
 * [ ] ANSWER: it's already supported, but in previous versions; to be cleaned up?
* [x] extreme thought: yes, this is an extreme thought: consider module search/install extensions!
 * [ ] ANSWER: or try it automatically by way of "module load" instead ;-)
* [x] promote easybuild within bioinformatics community, esp. for "clinical or translational workflows", as described below: http://www.bioinfo-core.org/index.php/11th_Discussion-7_November_2011#Topic_2:_Managing_and_Tracking_Software_Updates_.28Led_by_Brent_Richter.29
 * [x] ANSWER: makes sense to approach communities; TBD
* [x] support cmake-like builds (see example below)
 * [x] ANSWER: for the default case an easyblock is upcoming; TBD
* [x] how to override certain steps ("/bin/true" is considered a hack as of now)
 * [x] ANSWER: to be discussed; first create wiki page with all the [[Build ports naming conventions]]
* [x] Consider the bigger picture as seen at http://eniac.cyi.ac.cy/display/UserDoc/HPC+Baseline+Configuration
 * [x] ANSWER: discussed; interesting for Flamish supercomputing activities
* [x] BENELUX collab?
 * [x] ANSWER: discussed; TBD

# SYSADMIN-oriented features:

* [x] Define the *namespace* "by a standard"; eg. like [https://github.com/fgeorgatos/HPC-RFC/blob/master/0001/0001.md]
* [x] Give freedom to define the namespace format (eg. original Package names OR the lower case version)
 * [x] allow to export *namespace* in both UpperCase and LowerCase conventions # a post-install hook? TBD
* [x] Support more configurable formats of version strings (check below, how other HPC sites do it)
 * [x] nested levels (ie. split version string in sub sections) (yes, via #879)
 * [x] provide hooks for gnu/gcc/intel/pgi etc strings (custom-module-namespace allows whatever nowadays)
 * [x] IDEA: For now, supply a "custom_namespace_function" variable, to define eg. modulename.lower();
* [x] Provide some smart way to handle osdependencies like "tcsh" which may have no meaning on Debian # a particularly important case are the boost-devel vs boost-dev differences (rpm vs deb conventions)
 * [x] and for now allow escaping with --ignore-os-dependencies (--ignore-osdeps does this since ~ v1.10)

# Building GROMACS-GPU

```
# GROMACS-GPU uses a cmake build-generator and makefiles on unix.
# To avoid confusion build in separate folder

export OPENMM_ROOT_DIR=path_to_custom_openmm_installation
mkdir build_gromacs_gpu
cd build_gromacs_gpu
cmake PATH_TO_SOURCE_DIRECTORY -DGMX_OPENMM=ON -DGMX_THREADS=OFF
make mdrun
make install-mdrun
```

# modules namespace from various HPC sites

```
  ---------------------------------------------------------- --------------------- ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
  Example name                                               WHO                   Reference URL & Comments
  ---------------------------------------------------------- --------------------- ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
  netcdf/3.6.2                                               TACC                  [[http://services.tacc.utexas.edu/index.php/using-modules][]] \\ Lmod: automatic reloading of an entire module hierarchy when a single module anywhere in the hierarchy is changed.
  hpc/netcdf-3.6.3_intel-11.0.083                           HARVARD               [[http://rc.fas.harvard.edu/faq/modulelist][]] \\ very long and complete list
  misc-libs/netcdf/3.6.3_intel                              CLUMEQ                [[https://www.clumeq.ca/wiki/index.php/ModulesDisponiblesSurColosse][]] \\ nested layout
  netcdf/intel/64/4.0                                        SARA                  [[http://www.sara.nl/systems/shared/modules/][]] \\ [[http://www.sara.nl/systems/lisa/news/modules-ng][]] \\  [[http://www.sara.nl/systems/lisa/news/amd64-phase1][]] \\  [[http://www.sara.nl/systems/lisa/news/Modules-deprecated-2011-04-04][]]
[[https://www.clumeq.ca/wiki/index.php/ModulesDisponiblesSurColosse][]] \\ nested layout
  netcdf/gnu/64/4.0                                          SARA                  [[http://www.sara.nl/systems/shared/modules/][]] \\ [[http://www.sara.nl/systems/lisa/news/modules-ng][]] \\  [[http://www.sara.nl/systems/lisa/news/amd64-phase1][]] \\  [[http://www.sara.nl/systems/lisa/news/Modules-deprecated-2011-04-04][]]
  netcdf/4.0.1                                               UIBK                  [[http://www.uibk.ac.at/zid/systeme/hpc-systeme/common/tutorials/modules-howto.html][]] \\ PREFERRED_MC
  netcdf/X.Y.Z/gnu-4.1.2                                     UIBK, too             [[http://www.uibk.ac.at/th-physik/howto/hpc/modules.html][]]
  netcdf/X.Y.Z-gnu                                           CSC                   [[http://www.csc.fi/english/pages/hippu_guide/using_hippu/modules/index_html][]]
  ofed/qlogic/gcc/64/1.2.7                                   UCL                   [[http://www.ucl.ac.uk/isd/common/research-computing/services/legion-upgrade/userguide/userenvironment][]]
  netcdf/3.6.2                                               MIT                   [[http://www.darwinproject.mit.edu/wiki/index.php/Compute_cluster_hardware/software_overview][]]
  netcdf/X.Y.Z                                               MIT, too              [[http://coyote.mit.edu/mediawiki/index.php/Modules][]]
  netcdf/3.6.2                                               CAM                   [[http://www.hpc.cam.ac.uk/user/software.html][]]
  netcdf/4.0.1_nc3                                          UTORONTO              [[https://support.scinet.utoronto.ca/wiki/index.php/Software_and_Libraries][]]
  blas/?                                                     VLSCI@AU              [[http://www.vlsci.org.au/documentation/software-applications][]]
  fluent/?                                                   EPFL                  [[http://pleiades.epfl.ch/index.php?option=com_content&task=view&id=15&Itemid=30][]]
  netcdf/4.0.0.3.1jg64                                       CSCS                  [[http://user.cscs.ch/software_and_programming_environment/compilers_and_programming/rosa_cray_xt5/modules_framework/index.html][]]
  netcdf/4.1.1/pgi/10.3/64                                   TAMU                  [[http://brazos.tamu.edu/software/modules.html][]]
  sles10.1_gnu4.1.2_shared                                 UTENNESSEE            [[http://www.nics.tennessee.edu/computing-resources/kraken/software][]]
  netcdf/4.0-pgi, /4.0-gcc , /4.1.1                          UMICH                 [[http://cac.engin.umich.edu/resources/software/][]]
  netcdf-4.0.1                                               ARSC                  [[http://www.arsc.edu/support/news/systemnews/][]]
  NetCDF/4.1.3-gnu                                           Griffith University   [[http://confluence.rcs.griffith.edu.au:8080/display/GHPC/netcdf][]]
  HMMER                                                      iCER/HPCC             [[https://wiki.hpcc.msu.edu/display/Bioinfo/Module+Files][]]
  HMMER                                                      UPPNEX                [[https://www.uppnex.uu.se/installed-software][]]
  uberftp-client-2.6                                         NCSA                  [[http://www.ncsa.illinois.edu/UserInfo/Resources/Hardware/CommonDoc/module.html][]]
  netcdf                                                     LRZ                   [[http://www.lrz.de/services/software/utilities/modules/deisa_details.html][]] \\ Ref. on DEISA setup: [[http://www.lrz.de/services/software/utilities/modules/deisa_details.html][]]
  globus                                                     PRACE sites           [[http://www.prace-ri.eu/Interactive-Access-to-HPC][]]
  globus/5.0.4 & GLOBUS-5.0                                  TACC                  [[http://www.underworldproject.org/BuildRecipes/ranger.rst][]] See Teragrid below
  netcdf/4.1.2-gnu, /4.1.2-intel, /3.6.3-gnu, /3.6.3-intel   cyi/euclid@ls2        [[http://eniac.cyi.ac.cy/display/UserDoc/HPC+Baseline+Configuration]]
  netcdf/3.6.3-gcc, netcdf-4/4.0.1-gcclf                     cyi/planck   [[http://eniac.cyi.ac.cy/display/UserDoc/HPC+Baseline+Configuration]]         
  netcdf/3.6.2-intel, /3.6.2-gnu                             ba@ls2           [[http://eniac.cyi.ac.cy/display/UserDoc/HPC+Baseline+Configuration]]     
  netcdf/4.1.3-gcc(default)                                  cytera@ls2       [[http://eniac.cyi.ac.cy/display/UserDoc/HPC+Baseline+Configuration]]     
  netcdf/1.4.3-mpich2-intel-64bit                                  FZK       [[http://www.google.com]]     
  ---------------------------------------------------------- --------------------- ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
```

# CUDF Examples

```
package: m4
version: 3
depends: libc6 >= 8

package: openssl
version: 11
depends: libc6 >= 18, libssl0.9.8 >= 8, zlib1g >= 1
conflicts: ssleay < 1

package: gcc
version: 4.4.4
provides: compiler
conflicts: compiler

package: icc
version: 10.2
provides: compiler
conflicts: compiler
```
