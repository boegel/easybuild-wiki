* attending conf call: Dag Sverre Seljebotn, Jens Timmerman, Kenneth Hoste

#### HashDist in general

* goalf of HashDist: switch software stacks like you could switch git branches
* hash includes build configuration, dependencies, OS, ...
 * NIX/GNIX goes very far, they include *everything* in the hash
 * goal of HashDist is not that extreme
* software installations are done in prefix path that includes hash
* software stack is then a profile that combines a bunch of these packages together
 * updating is simply building what's missing and updating the profiles
 * previous builds are cached, HashDist detects what was built before and reuses those builds
* initial version will be specific to HPC, but intention is to support also OS X, Windows, ...

### HashDist and EasyBuild

* HashDist is not meant to be user-facing, but more intended like an internal package managing tool
 * e.g. for software like Sage that manages a whole bunch of (sub)packages itself
* in one sense, HashDist is more like a replacement for environment modules
* supporting HashDist in EasyBuild would be a bit more invasive than the support for environment modules
 * e.g. HashDist would want control over the installation prefix
 * easy to add by putting the required hooks in place in EasyBuild
 * EasyBuild could serve as an example of how to integrate HashDist into existing tools

#### summarized

HashDist is basically a list of 4 tools:
 
1. LD_PRELOAD tool to figure out whether system libraries not explicitely specified are being used for build
 * e.g. "LD_PRELOAD=jail.so gcc ..."
2. manager of db of installation prefixes based on hashes
3. make symlinked profiles that represent a software stack
 * more complex for e.g. Python
  * you would need to make the 'python' binary search for packages in paths other than the ones in the Python search path
  * by writing a wrapper around python, or wrapping around the loader that looks for the required packages
 * setting `LD_LIBRARY_PATH` may break stuf, especially on a desktop environment
  * e.g. printing to a printer from Python code, using a self-built Python distribution (e.g. using EasyBuild)
  * instead of setting `LD_LIBRARY_PATH`, you could link with `-Wl,-R${ORIGIN}/../../<hash>`
  * `${ORIGIN}` will be replaced with actual location of library, making stuff relocatable just like with setting LD_LIBRARY_PATH
4. an alternative for environment modules, that would also work in non-HPC environments

#### licensing
* GPLv2 license is an issue in the Python scientific software community, which is very much BSD
 * if EasyBuild wants to become the defacto standard in the Python community, reclicensing would probably be required
 * HashDist (support) would have to be in a separate repo
 * we should contact NUMfocus mailing list, poll whether how much GPLv2 licensing would matter for a tool that is just to be used, cfr. Linux kernel

#### related stuff
 * python-hpcmp: DoD effort to package bunch of Python packages
  * they basically have a huge Makefile together with some config files
  * https://github.com/erdc-cm/python-hpcmp
 * WAF: build tool that does configure/building of software
  * support for building numpy/scipy support is in development, as an alternative to distutils
  * but distutils probably won't disappear, too much legacy 
  * http://code.google.com/p/waf/