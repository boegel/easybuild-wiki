#### Markus: implementing a custom module naming scheme for a hierarchical scheme
 * currently missing in EasyBuild:
  * prepend to `$MODULEPATH` in generated compiler/mpi modules should be automagic
  * provide full parsed easyconfig file to module naming scheme module, so e.g. module class is accessible (see [related framework issue](https://github.com/hpcugent/easybuild-framework/issues/687))
  * need to provide `%(modulesroot)s`, allow specifying absolute paths via `modextrapaths` (`prepend_paths`)
  * generated modules use full module names, while they should only use the module names relative to the `$MODULEPATH` in a hierarchical setup...
  * see also https://github.com/hpcugent/easybuild-framework/issues/862



#### TODO
testing build of new Cube version with EB (using system provided deps)
     plan to create easyconfig files and store them in own repository, then issue pull request shortly before release
     Cube enforces --disable-gui if the provided Qt dependency is not OK silently (configure doesn't complain)
     missing in EB:
          SystemModule easy block?
          support in --robot for specifying URLs to remote easyconfig repos

write easyblock for installing Score-P dev tools in the same prefix (libtool, autoconf, …) after patching them
     issues with setting up 'production' and 'development' environments for writing own easyblocks/easyconfigs

install gmpich2 based on existing gcc/mpich3 modules, build FFTW on top of that
     missing in EB
          documentation or cmdline option on generating module for system-provided tools or existing modules

mkl testing tool with static build
     figure out problem with imkl include files not being found => add -I$EBROOTIMKL/mkl/include/intel64/lp64 to $FCFLAGS
     fix BLAS linking issues by defining $LIBS => makes sure that libraries are added at the end, which is a strict requirement when statically linking (cfr. http://software.intel.com/en-us/articles/static-linking-with-mkl-ipp-or-tbb-may-give-unresolved-references)
     building of examples is broken due to use of -ldl ?!?

Bernd: flesh out easyblock for Tau, and also look into HPC Toolkit (dep for PerfExpert)     
     lots of documentation available at JSC (Anke) on building perf tools (like HPC Toolkit)

hierarch modules, multi-user usage, get software installed via JSC policies (no write access to shared FS, now a try archaic packaging system), …
different groups look after different tools (compiler, math libs, IO libs, …), have to be provided to each other

Alan: look into multi-user setup
     properly set umask
     set sticky bit on top install directory
