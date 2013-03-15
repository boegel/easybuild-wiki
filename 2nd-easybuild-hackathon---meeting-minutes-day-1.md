(Thursday Nov. 29th 2012, 11am-8pm)

During the first day of the [[2nd EasyBuild hackathon]], EasyBuild was introduced by Kenneth to the attendees who were mostly unfamiliar with EasyBuild by means of the SC'12 presentation, and some discussions were held about open bug tickets and future directions of EasyBuild. Fotis briefly explained the idea behind the HPCBIOS project.

## Presentation

### EasyBuild: comments, suggestions, remarks

 * ATLAS: a really tuned build takes 36h (Nicolas)
  * ask Nicolas to contribute an easyconfig for this
 * have a `--dry-run` option to make EasyBuild spit out what it would do (environment, commands, etc.) (Xavier)
  * would help with reviewing easyblocks, e.g. OpenFOAM
 * `--resume` option, continue from a certain step (Xavier)
 * rewrite modules in Python (Fotis)
  * what's the big advantage here??
 * people in Greece are interested in contributing PGI support (Fotis)
 * how to set up easyblocks repo outside of EasyBuild? (Cedric)
  * see https://github.com/hpcugent/easybuild-framework/issues/119
 * suggestion by Fotis: get EasyBuild in Google Summer of Code

### HPCBIOS

 * various policies where HPC sites to which could conform

 * EasyBuild could be a very useful tool to allow them to conform

### webinar on parallel debuggers offered by Allinea DDT

 * TotalView vs Alinea DDT
 * TotalView has some nice features, e.g. scripting (in Tcl), replaying of execution, MemoryScape to debug memory allocation/deallocation, memory leaks, ...
 * Intel Inspector only follows a single process
 * Allinea DDT debugger webinar:
  * Allinea DDT: parallel debugger, Allinea MAP: enhance application performance (profiling tool)
  * DDT debugger seems very advanced, lots of options/views/info provided in a useable manner, looks very powerful for debugging parallel codes
  * see http://www.allinea.com/

## Discussathon

 * HPCBIOS policies could be easily complied to by using EasyBuild
  * if a (new) group of users comes up and desires compliance to a certain policy, the step could be fairly small
 * hpcbios.readthedocs.org
  * homework: review http://hpcbios.readthedocs.org/en/latest/HPCBIOS_2011-92.html
 * modules namespace discussion (https://github.com/hpcugent/easybuild-framework/issues/173), see http://hpcbios.readthedocs.org/en/latest/HPCBIOS_2011-91.html
   * we should stay with the current module naming scheme used by EasyBuild, and others that are specified by user should be symlinked (or via module aliases)
 * module classification (see https://github.com/hpcugent/easybuild-framework/issues/209)
  * agree on set of decent module classes (bio, chem, phys, ...)
 * installation of EasyBuild is too much of an issue
  * just provide a tarball that contains the full thing?!
  * revert quick demo to something a lot more basic, e.g. building gzip with the system compiler (and explain why it's not a good idea)
  * support `./eb --bootstrap` to allow EasyBuild to install itself, and try a bunch of different options (`--user`, `--prefix`, `easy_install`, `pip`, `python setup.py`, etc.)
 * TODO: go through all the issues, pick the ones that should have milestone v1.1
 * discuss issues currently marked 'high' priority
  * mainly documentation, and making module naming scheme flexible
