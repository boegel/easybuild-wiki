(Monday Mar. 11th 2013, 10am-6pm)

The first day of the [[3rd EasyBuild hackathon]] consisted of presentations, discussions and initial hands-on experience with EasyBuild for attendees new to the tool.
These notes were mainly taken by Kenneth and Jens, with contributions by Fotis.

## Attendees

 * Kenneth Hoste (HPC-UGent, EasyBuild developer and release manager)
 * Jens Timmerman (HPC-UGent, EasyBuild developer)
 * Fotis Georgatos (University of Luxembourg, HPC sysadmin and active contributor)
 * Jens Wiegand (The Cyprus Institute, LinkSCEEM project manager)
 * Thekla Loizou (The Cyprus Institute, HPC user support)
 * George Tsouloupas (The Cyprus Institute, HPC sysadmin and user support)
 * Stelios Erotokritou (The Cyprus Institute, HPC user support/PRACE)
 * Mohamed Gafaar (Bibliotheca Alexandrina, HPC sysadmin/user support)
 * Dina Mahmoud Ibrahim (Cairo University, HPC sysadmin/user support)
 * Alan O'Cais (Jülich Supercomputing Centre, HPC user support & LinkSCEEM)
 * Alexander Schnurpfeil (Jülich Supercomputing Center, HPC user suppot)
 * Nicolas Kanaris (The Cyprus Institute, HPC user (OpenFOAM))
 * George Fanourgakis (The Cyprus Institute, HPC user, molecular dynamics)
 * Demetris Charalambous (Cyprus Meteorological Servicei, HPC user support?, weather forcecasting (WRF, ...))
 * Ioanna Kalvari (University of Cyprus (bioinformatics), HPC user)
 * Ioannis Kirmitzoglou (University of Cyprus (bioinformatics), HPC user)
 * Adam DeConinck (NVIDIA Corporation, HPC sysadmin) [remote via Skype]

## Program

 * [10am-10.10am] presentation on LinkSCEEM project
 * [10.10am-10.15am] introduction round: who's who?
 * [10.15am-12am] presentation on EasyBuild: Building Software With Ease
 * [1.30pm - 1.45pm] presentation by Jülich Supercomputing Cente (JSC) on current activities and plans with EasyBuild
 * [1.45pm - 2.30pm] presentation by Cyprus Institute on current activities and plans with EasyBuild
 * [2.30pm - 5pm] discussions + initial hands-on experience with EasyBuild
 * [5pm - 6pm] presentation by NVIDIA: Introduction to the CUDA Toolkit for Building Applications
 * [6pm - 8pm] aftermath: discussions w.r.t. CUDA support in EasyBuild

## Presentation notes

### LinkSCEEM presentation (Jens W.)

 * goal of LinkSCEEM project: establish HPC ecosystem in Eastern Middle-Terranean
  * resources, training, expertise, connectivity, ...
   * online training content is very important!
  * with support from NCSA, Jülich, ..
 * www.isgtw.org
 * CSC2013 PRACE conference in Cyprus
 * Alan's subject: performance analysis and optimisation of community codes

### EasyBuild presentation

[slides (PDF)](http://hpcugent.github.com/easybuild/files/easybuild_hackathon_Cyprus_20130311.pdf)

#### questions/remarks by George T.

 * `--download-only` command line option is missing (see [framework#538](https://github.com/hpcugent/easybuild-framework/issues/538))
  * can be done indirectly now:
   * using `--stop fetch`, but will fail after first download failed
   * using `--regtest`, and 'breaking' `--job` so no jobs are submitted for the builds
 * EasyBuild bootstrap script **[question]**
  * why not include fixed Python 2.7 version in bootstrap procedure, so we have full control of Python version being used
   * add Python module as a dependency for EasyBuild
 * why does something have to be part of a toolchain, and not just a dependency? **[question]**
  * can toolchain be extended dynamically?
   * e.g. include dependencies in toolchain as well?
   * `ictce` => `ictcee` (`e` for extended)?
  * creating yet another toolchain means that whole stack of dependencies needs to be rebuilt, which is a pain **[question]**
   * and results in further explosion of the set of available modules
  * create big fat toolchains and filter stuff out in `toolchainopts` with `filter` option?
 * are existing modules reused if they're needed? **[answer: yes]**

#### questions/remarks by Fotis

 * supporting alternative module naming schemes (see [framework#173](https://github.com/hpcugent/easybuild-framework/issues/173))
  * basically just provide multiple alternative views on the existing modules - let's escape from the "one size show" concept
   * flat (cray optimal) or hierarchical (lmod optimal), all can be valid
   * hierarchical can be top-down (compiler->libraries->apps) or vice versa (software on top, that is what the users care about)
 * setting up mirrors for sources
  * initial mirror prototype already in development/use at Uni.Lu; ~36GBs of software (open source & closed)
  * The split between redistributable and non-, is unavoidable for a public service; zsync could help with either
  * `--try-amend` source URL should be supported via EasyBuild configuration file (see [framework#462](https://github.com/hpcugent/easybuild-framework/issues/462))
   * **currently already supported via `EASYBUILD_TRY_AMEND` env var**
 * Trilinos and other such multi-dep libs, should be in bold on slide with supported software
 * can `build_in_install_dir` be specified in easyconfig file? It might reduce needs for easyblocks in bioinfo packages (see [framework#539](https://github.com/hpcugent/easybuild-framework/issues/539))
 * `ictce/3.2.2.u3` toolchain sources are no longer available, so use other toolchain in examples (WRF)
  * also there are bugs in icc/11.1.07* & flexlm, which are time-consuming to debug (EB processes *are* affected)
 * chroot/jail into installation prefix, as a proper containment solution when building software: (see [framework#540](https://github.com/hpcugent/easybuild-framework/issues/540))
  * it makes it much safer to build 1000s of packages coming from pkgsrc (at least, at build time!)
  * it will allow to catch osdependencies that now escape unnoticed
  * it seems to be the correct thing to do also in relation to hashdist
  * Fotis: no need to build safety features inside easyconfigs, IMHO, that has no clear benefit
 * possible directions on how to modify the current namespace (eg. think of lower-case modules):
  * customize modulefiles with sed and change what is needed (can be tricky or risky to be correct)
  * manually cultivate a symlink farm (only good for the first load, deps will be like before)

#### questions/remarks by Mohamed

 * can EasyBuild cope with existing modules (e.g., OpenMPI), or do they need to be rebuilt?
  * will not work out of the box, because those modules will be missing things like EBROOT, EBVERSION
  * rebuilding them is the best idea, and will enable you to roll out software again after reinstall of system with different OS

#### questions/remarks by Alan

 * adding support for BlueGene Q system
  * perfect timeframe since it's quite new
  * specific characteristic: crosscompilation, IBM XLC compiler, ...
 * on BlueGene systems, running of tests will need to be skipped or done differently (remotely)
  * skipping can be done with e.g. `--try-amend=skipsteps=test,test_cases`
 * `OpenFOAM` is a pain because of large difference in system characteristics
 * error/warning log parser now spits out lots of false positives (see [framework#541](https://github.com/hpcugent/easybuild-framework/issues/541))
  * regular expression used needs to be documented well
  * need to enhance regex to reduce amount of false positives
 * `bbcp` can never work if the required ports are not open
  * add a test case for this?
  * support a way of spitting out a warning about this at the end of the installation (see [framework#542](https://github.com/hpcugent/easybuild-framework/issues/542))

#### notes by Jens T.

 * configuration file (and .eb files) will not be executed anymore => needs to be followed up (w/ Stelios)
 * more stuff needs to be shoved into toolchains: Python (George T.), zlib, ...

#### general notes

 * document where to put source files **(see [wiki:Configuration](https://github.com/hpcugent/easybuild/wiki/Configuration))**

### Presentation by JSC (Alan)

 * supercomputing training portal: http://linksceem.eu/ls2/component/content/article/198
  * big fat training cluster via VMs w/ terminal emulation in web browser
  * rely on EasyBuild to get software installed on this
 * should function well in a low-bandwidth environment (important for LinkSCEEM)
 * documentation portal w.r.t. supercomputers, just collect links to useful documentation around the web
 * try and get a toolchain working for BlueGene Q systems
 * categories of software used in PRACE

### Presentation by Cyprus Institute (George T./Fotis G.)

N.B. This presentation incorporates also the 3rd-party needs of Uni.Lu

 * strong commitment from Cyprus Institute to EasyBuild
 * was really useful to quickly set up a software stack
  * GPU software, even though not all apps were there
 * useful for conformity across LinkSCEEM institutions - very important
 * also for setting up post-processing nodes (w/ different OS)
 * robot should not depend on filename, but on contents of easyconfig
 * need support for multiple source paths (and dependencies? => Jens T.)
 * `--download-only` (from mirrors), needed in relation to jumpstart procedures
 * document jail tool (and add it to bootstrap)
 * goolf: OpenMPI >=1.5, OpenBLAS, FFTW w/ `--enable-avx`
 * custom variables in module files (**see** `modextravars`)
 * CUDA support: in toolchain or not?
  * different CUDA versions, dependency chain, ...
 * PGI toolchain is also important for CyI
 * OFED vs no-OFED: easy way to get rid of `-no-OFED` version suffix? via EB config file?
 * user environment: multiple source paths, custom version suffix for 'tagging' your own builds
 * FFTW single/double precision
  * separate module (and thus separate toolchains) vs 'fat' build
  * support both single/double in a single FFTW module requires running configure/make/make install twice; doable? Yes.
 * local climate group requirements: Ferret, ... (see George F.)
 * Python as a part of the toolchain?
 * managing multiple EB versions
  * need a guidelines and best practices for EasyBuild
  * what should sites customize? what should be left untouched (e.g. becaue it'll break in the future with new EB versions) 
 * how to override default use of `/tmp` (**set `$TMPDIR`, which will be picked up by Python**)
 * explosion of available modules
  * we need a good way to handle that
  * in combination with flexible module naming scheme?
 * figure out exact build options that were used
  * document querying log for e.g. configure options
  * or provide tools for it (via `eb`)
  * will be useful in the future as part of any kind of `EasyDoc` activity, to post information on a website
 * issue of OS dependencies: portable way of specifying them
  * making sure we catch all dependencies (**jail tool provided by HashDist**)
 * `goalf`/`ictce` versioning schemes need to be documented
  * e.g. add `--enable-avx` to FFTW but keep toolchain version the same? 
   * sidenote: `--enable-avx` with FFTW apparently is suboptimal for GROMACS, up to 20% perf loss (!)
   * see http://www.gromacs.org/Documentation/Installation_Instructions#3.2.1._Running_in_parallel
  * will need to bump ATLAS anyway (because of Sandy Bridge support), hence `goalf` as well
  * keep versions fixed but tweaking builds is not a good idea
 * OpenMPI version: let's bump to v1.5, or later
  * ABI is *guaranteed* to be compatible at version onwards (ie. change on the fly the OpenMPI module version, without need to rebuild code)
  * part of new goalf (v2.x?)
 * GROMACS on top of new `goalf` (`goolf`)
 * (Fotis took over for the remainder)
 * PRACE production environment
  * largely done; mainly a set of environment variables is missing (JT: could use `modextravars` for that)
  * EasyBuild enables you to set up a PRACE environment in user space (huge selling point: you only need GCC/EnvMod/Py)
 * support for custom site-specific environment variables
  * fi. *_LICENSE_FILE, DEBUGGERS environment and so on;
  * see `modextravars` stated above
 * HPCBIOS pitch: ie. a standardization effort, collection of policies, see http://hpcbios.readthedocs.org/en/latest/
  * re-usable high-level documentation (intention is to attach it as .pdf to Users' Welcome Letters)
  * provide "standard" working environment for e.g. climate science

### NVIDIA presentation (Adam) 

[slides (PDF)](http://hpcugent.github.com/easybuild/files/CUDA_Toolkit_for_Sysadmins.pdf)

 * [Alan] documentation for building CUDA applications provided by NVIDIA is very useful and hard to come by!
 * NVIDIA CUDA with OpenMPI: K20 + Mellanox IB
 * 'drop-in' libraries: cuBLAS
  * actually a misnomer, since it provides different function names
 * CUDA (C)
  * compilers + tools
   * `nvcc` should always be used, unlike `mpicc` which is optional (can handle linking with MPI libs yourself)
  * runtime API (`libcudart.so`)
  * IDE, visual profiler
  * collection of libraries (CUBLAS, CUFFT, Thrust, ...)
  * quite similar to e.g. MPI (?)
 * usually there's a configure option like `--enable-cuda`, but no standard
  * similar for installation prefix for CUDA, e.g. `CUDA_HOME` (quite similar across apps)
 * some apps ship their own runtime
  * be careful with setting `LD_LIBRARY_PATH` in a CUDA context
 * `nvcc` treats C code like C++ (!)
 * `-use-fast-math` mostly targets single-precision stuff
  * can be broken up into parts
 * `-Xptxas=iv` can always be set for having debug info
 * most MPI implementations support CUDA (except for Intel MPI)
  * things may break for older versions
  * significant performance gain if communication layer supports DMA to GPU memory (GPU Direct, requires IB QDR/DFR and Mellanox)
  * FG sidenote: ie. MVAPICH + CUDA is the likely the preferred testing ground
 * test examples: `matrixMul`, `simpleMPI`
 * CMake 2.8.7+ for good CUDA support
 * object code for correct device architecture should be used for best performance
  * make sure PTX is not being used, which is JIT-compiled and thus leads to startup performance loss
  * default is to target oldest PTX/object code: `--gencode` default to `compute_10,code=sm_10`
  * usually something is set in the application Makefile
  * `OPENCCFLAGS=gencode...`
  * `PTXARGS=--arch,...`
 * can CUDA toolkit be queried for what options are optimal for current device architecture?
 * possible options for `gencode` depend on CUDA compiler version
 * build for "everything under the sun"
  * will fail on older `nvcc` systems
  * larger binary
 * OpenACC: very similar to OpenMP (only PGI, Cray, CAPS compilers for now)
  * for PGI:
   * `-acc` -> use OpenACC
   * `-Minfo=accel` -> compile for accelerator (not CPU, which is also possible)
   * `-ta=nvidia` (vs AMD or Intel accelerators)
   * default architectures targetted are current GPU + major versions (1.0, 2.0, 3.0),
     but can be tuned via command line options
 * CUDA compiler commands depend on compiler being used, e.g. `pgfortran` (PGI) for CUDA Fortran
 * `nvcc` is actually an LLVM frontend
  * developer.nvidia.com/llvm
  * http://docs.nvidia.com
  * http://developer.nvidia.com/nvidia-registered-developer-program to get GPUs
  * adeconinck [at] nvidia.com for questions
 * follow-up conf. call
  * packaging of CUDA toolkit (`redhat`, `fedora`, `ubuntu`, ...)
   * reason to not have a monolitic install is OS-specific stuff like paths for files, driver, etc.
   * "let user provide CUDA toolkit installation instead of through EasyBuild" (**not a good idea**)
    * FG sidenote: optimal practice @LU & @CY converges in the direction that:
     * CUDA toolkit & software installation is user space => this has to be done with easybuild, multiple versions OK.
     * driver installation must be done in root space => non-easybuild business, configuration management instead
   * use `-silent` for only toolkit (not driver)
   * OS-independent install package is being looked into
  * can notes for NAMD be provided as well?
  * CUDA vs Xeon Phi build process
   * Xeon Phi is via magic options in Intel compilers (cfr. PGI)
   * different modes: native mode (all on Xeon Phi), offload mode (host + Phi as accelerator), OpenACC (via pragmas in code), x86-only (run x86 binary on Phi)
 * open questions
  * standard variables for CUDA compilers (e.g. `CUDA_CC`)
  * Intel compilers + CUDA?
  * which `gencode` options should be set for CUDA-enabled toolchain?
  * performance issues with 'fat' binaries (multiple device architectures) due to instruction cache bottleneck?
  * can compiled binary be queried for which options were used? (George T.)

## Hackathon notes

 * environment modules may be Tcl-only version, which only provides `modulecmd.tcl`
  * EasyBuild needs to be able to handle that
 * `modulecmd` may not be in path, but hardcoded in `module`
 * make bootstrap script work offline too, i.e. add option to supply it the required source tarballs
 * goolf v1.5.10
  * GCC 4.7.2
  * OpenMPI >=1.6.3 (1.7rc8 not production ready + requires GCC > v4.8!)
  * OpenBLAS 0.2.6
  * LAPACK 3.4.2
  * FFTW 3.3.3 (single/double)
  * ScaLAPACK 2.0.2
 * problems with EasyBuild bootstrap script during [training exercises](http://hpcugent.github.com/easybuild/files/EasyBuild_training_exercises_Cyprus_hackathon_2013.pdf)
  * use `modulecmd help` instead of `-H` (latter doesn't work with Tcl environment modules?)
  * warn about installing with root
  * `lib` vs `lib64`
  * offline mode for bootstrap script required, e.g. login nodes can not go online (Mohamed)
   * but workernodes can :)