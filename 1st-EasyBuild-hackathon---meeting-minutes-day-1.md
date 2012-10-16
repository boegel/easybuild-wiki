(Thursday Aug. 16th 2012, 10am-7pm)

During the first day of the [[1st EasyBuild hackathon]], mostly discussions were held. These notes were taken by Kenneth, and edited by Fotis.

## Attendees

introduction round:
 * Stijn De Weirdt: HPC-UGent team lead, started EasyBuild
 * Fotis Georgatos: 
  * stumbled onto EasyBuild via HEPIX'2012 agenda, in a period he was looking for a tool like this (but didn't expect to find it in HEPIX!)
  * Cedric (clueful hpc user at Fotis' site) has been using it to build over 20 bioinformatics software packages, without much trouble
 * Jens Timmerman: has been extending EasyBuild in the last couple of months
 * Toon Willems: summer intern for EasyBuild, has been working on features and redesign for last month
 * Kenneth Hoste: lead EasyBuild developer during last year

## Discussion topics

### Development timeline

Brief overview of development timeline:
 * v0.9 by end of Aug. 2012
 * v1.0 by end of Sept. 2012
 * v1.1 by end of 2012
 * long-term v2.0 and v3.0 milestones already defined, no date set yet

### Open bug tickets

Went through open bug tickets (in internal Trac bug tracker), discussed some of them:
 * several ones could be closed already, e.g. dumping dependency graph, failing to build particular software packages, ... (**action point _KH_**) 
  * **FIXED**: cleaned up tickets, all open tickets moved to [GitHub issue tracker](https://github.com/hpcugent/easybuild/issues?state=open)
 * discuss security issues w.r.t. exec'ing easyconfig files **(see [#129](https://github.com/hpcugent/easybuild/issues/129))**
  * _(FG)_ not really a security issues per se, there are others ways to do nasty stuff (e.g. in easyblock, or through patch files)
  * _(TW)_ avoid being able to use import statements by emptying \_\_builtins\_\_ (see internal Trac ticket nr. 212)

### Discuss changes in v0.9

 * discuss more generic way of handling os dependencies **(see [#174](https://github.com/hpcugent/easybuild/issues/174))**
  * package names differ between OSs
  * now support for running commands to check whether functionality is available is already there
  * _(SDW)_ should not go outside of package manager, ask package manager for checking commands, command version, files being there, ...
   * support different package managers in EasyBuild (rpm/yum, dpkg, ...) to be implemented
   * or through meta modules 
  * _(FG)_: ie. meta-modules may correspond to policies, as seen here:
   * http://eniac.cyi.ac.cy/display/UserDoc/HPC+Baseline+Configuration and here:
   * http://www.ccac.hpc.mil/consolidated/bc/policy.php
   * example: "tcsh" can be provided via "LS2_05-06" or "FY05-06"; names are fictional they can be anything; they could also correspond to rpm/deb packages

 * easyconfig repository
  * _(FG)_ make EasyBuild check the repository to look for easyconfig files **(see [#207](https://github.com/hpcugent/easybuild/issues/207))**
  * _(FG)_ encourage people to share easyconfig files by adding some incentive if they contribute easyconfigs that worked back to the community
   * _(FG)_ will create an easy-going easybuild.experimental repository for people to play with ports and beyond; idea: give incentives with stickers/t-shirts/towels/whatever makes you comfortable with, for 1/10/100/1000 .eb contributions
  * _(JT)_ automatically create a pull request after a worked build **(see [#208](https://github.com/hpcugent/easybuild/issues/208))**

 * devel modules **(see [#175](https://github.com/hpcugent/easybuild/issues/175))**
  * (FG) support installing devel modules in a dedicated devel-modules MODULEPATH place, or both (also in install path)

 * checking for loaded modules created by EasyBuild by looking for (SOFT | EB)ROOT env vars **(see [#153](https://github.com/hpcugent/easybuild/issues/153))**
  * _(FG)_ add EasyBuild option to just ignore that step, if desired for special reasons (eg. massive build of packages where some may work anyhow)

 * _(FG)_ specify the exact policy on branch names (e.g. <BUG_TICKET>_<DESCRIPTION>) ; hopefully something that gives the headline information
  * **(action point _KH_)** 
  * **_FIXED_**: see [[Contributing back]]

 * _(FG)_ make toolkit support more modular **(see [#176](https://github.com/hpcugent/easybuild/issues/176))**
  * create classes for compilers, MPI libs, ...

### Experience report by FG

 * EasyBuild could be very useful for users as well
  *  build (new versions of) software themselves, test it thoroughly before it gets installed system-wide by sysadmins or, group-managers (eg. per scientific community)
 * overall fairly positive experience so far
 * easy to get started with EasyBuild based on "demo for the impatient"

### Public bug tracker

 * GitHub issue tracker
  * lacks bug hierarchy, blocking marks, ...
  * let's just try it
   * seems to be feature-rich enough (milestones, assignees, labels, reference to issue in commit messages, ...)
   * migrate all open Trac tickets to hpcugent GitHub issue trackers (**action point _KH_**) 
    * see https://github.com/trustmaster/trac2github/
    * **FIXED**: cleaned up tickets, all open tickets moved to [GitHub issue tracker](https://github.com/hpcugent/easybuild/issues?state=open) (manually)

### FGs wishlist

**action point _KH_ and _FG_**: issues need to be opened for these, and mentioned here

 * need to create EasyBuild config option to specify dir for temporary log dir **(see [#83](https://github.com/hpcugent/easybuild/issues/83), [#84](https://github.com/hpcugent/easybuild/issues/84))**
 * consider a standardized way of handle dependencies (eg. CUDF) so that the resolver can be an external tool  **(see [#210](https://github.com/hpcugent/easybuild/issues/210))**
  * support something like a CUDF function to output easyconfig in CUDF format; 1 idea is to embed it in the easyconfig source
  * allow to override current dependency solver with something different (e.g. CUDF based)
 * support more freedom in having alternative namespace(s) for modules; this is badly needed for most big operating HPC sites **(see [#173](https://github.com/hpcugent/easybuild/issues/173))**
  * _(FG)_ killer feature for v1.0 so that existing documentation and user conventions does not have to be broken; eg.: lower-case module namespace
  * create a symlink farm with given alternative names to modules files created by EasyBuild
   * allow sysadmins to define function (or Python module) to map easyconfig parameters to this alternative namespace
   * try and come up with documentation that shows how to implement different namespaces
   * issues with dependency resolving by modules, conflicts between modules can be fixed by making modules more complex (if-else-if constructs for loading dependencies)
    * _(TW)_ or maybe my 'alias' feature offered by environment modules?
   * also release tool to merge from one namespace to another
 * categories of modules **(see [#209](https://github.com/hpcugent/easybuild/issues/209))**
  * already supported by moduleclass, except:
   * list of valid module classes is now hard coded, should be able to extend these in `easybuild_config.py`
   * specifying a list of module classes should also be supported
   * also support subcategories, e.g. `bioinformatics/tools`, `bioinformatics/apps`, `bioinformatics/datasets`
 * make a-z easyblocks hierarchy customizable **(see [#87](https://github.com/hpcugent/easybuild/issues/87))**
  * we should define a single namespace `easyblocks`, and then others can extend that namespace in any way they want (**action point _JT_**)
 * add support to robot for multiple paths to look for easyconfigs **(see [#211](https://github.com/hpcugent/easybuild/issues/211))**
  * and use regex to match .eb file names, together with os.walk
 * handling of non-`.eb` files in a modular way. eg: .rpm, .srpm, .spec, .deb, .py etc **(see [#212](https://github.com/hpcugent/easybuild/issues/212))**
  * so someone else can write a function/class for handling e.g. .spec files (see rpm Python module, https://bugzilla.redhat.com/show_bug.cgi?id=662119#c3) or source RPMs
 * add support for generating RPMs with EasyBuild **(see [#200](https://github.com/hpcugent/easybuild/issues/200))**
 * create a 'default' module that installs software that's not available yet ;-)
  * "module load software/not_yet" => "Installing software version not_yet, please hold..."
 * promote EasyBuild in BioInformatics community **(see [#120](https://github.com/hpcugent/easybuild/issues/120))**
  * big need and good fit because of requirement for reproducibility of experiments (software versions, etc.) AND multiple relatively small packages
 * next project: `easydoc`, to organize documentation on scientific software
  * e.g. make it produce HTML pages like http://eniac.cyi.ac.cy/display/UserDoc/GROMACS
  * format to write documentation in: MarkDownor so; _(JT)_PanDoc can help here for generality in output
  * idea is to allow community to organize documentation about scientific software centrally
 * add support for skipping particular steps in the build process **(see [#213](https://github.com/hpcugent/easybuild/issues/213))**
  * i.e. add support for a `skipsteps=[..., ...]` easyconfig parameter
  * so that implementing easyblocks just to skip steps can be avoided completely
   * and that `preconfigopts="/bin/true &&"` hack is not needed any more
 * add support for checking checksum of source files (option for source) **(see [#213](https://github.com/hpcugent/easybuild/issues/213))**
  * make checksum part of dumped easyconfig file in repository; eg. source_checksum=('sha1','2fd4e1c6 7a2d28fc ed849ee1 bb76e739 1b93eb12') or source_checksum='sha1:2fd4e1c6 7a2d28fc ed849ee1 bb76e739 1b93eb12'
 * arrows in dependency graphs should be reversed **_(KH)_: FIXED**
  * see http://raftaman.net/wordpress/wp-content/uploads/2010/07/firefox.png
 * rename some functions, e.g. make to build, source to fetch, ...  **(see [#99](https://github.com/hpcugent/easybuild/issues/99))**
  * figure out community consensus (see e.g. MacPorts)
  * _FG_ will create table to see if there's any kind of consensus on this
  * should make changes soon, as this will break existing easyblocks
 * make it possible to select how to configure, install, ... from within an easyconfig file **(see [#215](https://github.com/hpcugent/easybuild/issues/215))**
  * e.g. `easyblock = {'configure': "git_fetch", 'install': "binary"}`
  * pull these from the framework part of EasyBuild, figure out a way to make it extensible
 * see http://eniac.cyi.ac.cy/display/UserDoc/HPC+Baseline+Configuration for inspiration of list of software to support in EasyBuild
  * and for policies for HPC sites to comply to w.r.t. software/support
 * discussion about coming up with an encoding of package name to module/class name that avoids clashing **(see [#86](https://github.com/hpcugent/easybuild/issues/86))**
  * module names should be case-insensitive, to comply with PEP008 and support OSs like Windows and case-insensitive OS X HFS filesystems
  * class names should be unique (so case-sensitive)
  * encoding should only be used in case of clashes
  * we should have an academic answer to the academic question
  * **action point _SDW_**: come up with a documented encoding approach to be used in case of clashes
 * allow to specify default versions of selected software packages in EasyBuild config file  **(see [#219](https://github.com/hpcugent/easybuild/issues/219))**
  * e.g. for Python, add an entry that Python/2.7.2 should be the default (until we change it there)
 * add extra `installtarget` easyconfig option, to specify a different make install target **(see [#220](https://github.com/hpcugent/easybuild/issues/220))**
  * default value should be `"install"`
 * support specifying conflicts in easyconfig files **(see [#221](https://github.com/hpcugent/easybuild/issues/221))**
  * with version numbers, e.g. openssl v11 conflicts with "ssleay < v1"
  * idea: employ CUDF format style directly for this
 * create an easybuild-experimental repository to house generated easyconfig files (and maybe even easyblocks) **(see [#222](https://github.com/hpcugent/easybuild/issues/222))**
  * from pkgsrc/ports/portage
  * _FG_ will set this up, will be linked from EasyBuild main repo
 * specify that public contributions should be made under an OSI-approved license (requirement: open-source, redistributable) **(see [#223](https://github.com/hpcugent/easybuild/issues/223))**
  * adjust LICENSE file as needed to clarify that different code files may have different licenses
  * **action point _KH_**