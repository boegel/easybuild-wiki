(back to [[Conference calls]])

Notes on the 8th EasyBuild conference call, Tuesday Feb. 25th 2014 (10pm - 10.40pm CET)

 * follow link in "Video calls" section on [EasyBuild community Google+ page](https://plus.google.com/communities/103632287931200436158)

#### Attendees

Alphabetical list of attendees (7):

* Geert Jan Bex (UHasselt (VSC partner), Belgium)
* Pablo Escobar (University of Basel, Switzerland)
* Fotis Georgatos (University of Luxembourg)
* Kenneth Hoste (HPC-UGent, Belgium)
* Peter Maxwell (NeSI, New Zealand)
* Robert McLay (Lmod developer, TACC - University of Texas at Austin)
* Ward Poelmans (UGent, Belgium)

#### Agenda

* issues w.r.t. multiple EasyBuild users
* integration with Lmod and SLURM
* best way of creating/changing Makefiles (e.g. OpenSees)

#### Notes

##### Issues w.r.t. multiple EasyBuild users

* Jordi: sharing an install target with multiple EasyBuild users leads to problems
 * conflicts, ACLs, permissions
* Kenneth: this was solved at JSC by:
 * setting umask properly
 * using a sticky bit on the top install directory such that new files/directories have the required permissions
* Geert Jan: solved at UHasselt by restricting access to only people who are in a dedicated EasyBuild group, and then providing the group write permissions on the directory (`775` policy)
* problems are external to EasyBuild
* we should create a dedicated wiki page that describes solutions to problems related to sharing an install target

##### Integration with Lmod

* Peter: wondering about integration of Lmod and EasyBuild, goal would be to provide users with nice short module names
* Kenneth: some work was done last week during the hackathon @ JSC on using a hierarchical module naming scheme
 * cfr. https://github.com/hpcugent/easybuild-framework/issues/862
 * better support for hierarchical module naming schemes coming up in EB v1.12 (hopefully)
* remarks Robert
 * Lmod does not (and will not) support multiple versions in module names (referring to post by Peter Maxwell on Lmod mailing list)
 * anything beyond compiler and MPI library is set as a prerequisite (e.g. OpenBLAS, etc.), but only one BLAS/LAPACK is used for a particular software package at TACC
 * matrix support (full equivalent of toolchains in EasyBuild) for Lmod is being thought through
 * definition of toolchains is not clear, more/better examples are required to make things click
 * one issue is that there's no 1-to-1 mapping of BLAS/LAPACK/FFTW modules, e.g. OpenBLAS+FFTW vs Intel MKL
 * a new way for generating a module file from a script that should be sourced is available, after `env2` turned out to be (too) broken, cfr. `sh_to_modulefile` Lmod command (available in Lmod v5.3.1 and up)

##### Integration with SLURM (`--job`)

* Jordi: initial steps done in https://github.com/jordiblasco/easybuild-framework/blob/slurm/easybuild/tools/parallelbuild.py
* Kenneth: `pbs_job.py` should be thrown out, support for submitting jobs should be rewritten
 * abstract class `ResourceManager` with subclasses `Pbs`, `Slurm`, etc.
 * main method something like `submit_job(script, job_deps=[], submit_args='')`
* Jordi/Fotis: support for submitting to a particular partition would be nice
* Fotis: having a mechanism to specify job features based on software name or toolchain would be nice
 * goolfc => GPGPU nodes, ATLAS => full node build required due to autotuning
* Robert: Lmod supports the notion of attributes in module files, e.g. to indicate certain characteristics of the software being provided (e.g. MIC-enabled)

##### Install target: local directory vs shared filesystem

_(spinoff of SLURM discussion)_

* Robert: TACC packages builds in RPMs on dedicated build hosts, that are then deployed to the production systems
 * providing software through a shared filesystem would not work on a system the size of Stampede
 * exposing (new) builds directly to the users might not be a good idea
 * modules can be installed 'hidden', i.e. name starts with a `.` (e.g. `.foo/1.0`): they will not show up in `module avail`, but can be loaded
* Fotis: one other way (used at Uni.lu) is via buildsets, i.e. new install targets being prepared, actual switch to production happens during maintenance
 * users are able to use the next future production buildset early on, and are able to fall back on an old one


##### Best way of creating/changing Makefiles

* Jordi: best way to deal with software packages that require modifying or generating a `Makefile` (or `Makefile.def` to be imported in a `Makefile`)?
 * discussion raised because of PR for OpenSees easyblock, current implementation isn't exactly 'clean'
 * also required for e.g. VASP, LAMMPS, ...
 * cfr. https://github.com/hpcugent/easybuild-easyblocks/pull/324
* Kenneth: a couple of options:
 * runtime patching of an existing `Makefile` via `fileinput`, cfr. `build_step` in CP2K easyblock (https://github.com/hpcugent/easybuild-easyblocks/blob/master/easybuild/easyblocks/c/cp2k.py)
 * defining `makeopts`, and overriding whatever settings are in the `Makefile`, equivalent to e.g. `make CC=icc CFLAGS="-O3 -xHOST"` (cfr. https://github.com/hpcugent/easybuild-easyblocks/blob/master/easybuild/easyblocks/c/cblas.py)