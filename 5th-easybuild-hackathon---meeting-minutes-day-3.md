[[5th easybuild hackathon]] - [notes day 1](https://github.com/hpcugent/easybuild/wiki/5th-easybuild-hackathon---meeting-minutes-day-1) - [notes day 2](https://github.com/hpcugent/easybuild/wiki/5th-easybuild-hackathon---meeting-minutes-day-2)

### Discussion about current status of easyconfig format v2

bikeshedding discussion about dependency specifications in format v2


#### format v1 (for reference)

```
dependencies = [
    ('GCC', '4.8.1'),
    ('OpenMPI', '1.6.4', '', ('GCC', '4.7.2'),
    ('OpenBLAS', '0.2.6', '-LAPACK-3.4.2', ('gompi', '1.4.10')),
    ('FFTW', '3.3.3', '', ('gompi', '1.4.10')),
    ('ScaLAPACK', '2.0.2', '-OpenBLAS-0.2.6-LAPACK-3.4.2', ('gompi', '1.4.10')),
]

moduleclass = tools
```

#### format v2 (current format)

```
moduleclass = tools

[DEPENDENCIES]
GCC = 4.7.2
OpenMPI = 1.6.4; GCC == 4.7.2
OpenBLAS = 0.2.6 suffix:-LAPACK-3.4.2; gompi == 1.4.10
FFTW = 3.3.3; gompi == 1.4.10
ScaLAPACK = 2.0.2 suffix:-OpenBLAS-0.2.6-LAPACK-3.4.2; gompi == 1.4.10
```

#### brainstorm results

* single-line dependency specification (no dedicated section for dependencies):

```
dependencies = GCC(4.8.1), OpenMPI(1.6.5, gompi == 1.4.10), gompi(1.1.10, suffix:-no-OFED), OpenBLAS(>= 0.2.6, suffix:-LAPACK-3.4.2, goalf == 1.1.10 suffix:-no-OFED)
```

* multi-line dependency specification (no dedicated section for dependencies):

```
dependencies =
    GCC(4.7.2),
    OpenMPI(1.6.4, GCC == 4.7.2),
    gompi(1.1.10, suffix:-no-OFED),
    OpenBLAS(>= 0.2.6, suffix:-LAPACK-3.4.2, goalf == 1.1.10 suffix:-no-OFED),
    ...
```

* format v2 (proposed format for dependencies section)

```
moduleclass = tools

[DEPENDENCIES]
GCC(4.7.2)
OpenMPI(1.6.4, GCC == 4.7.2)
OpenBLAS(>= 0.2.6, suffix:-LAPACK-3.4.2, gompi == 1.1.10 suffix:-no-OFED)
...
```

* alternative (using keys for all entries, for consistency)

```
[DEPENDENCIES]
...
OpenBLAS(version: >= 0.2.6, suffix:-LAPACK-3.4.2, toolchain: (goalf == 1.1.10 suffix:-no-OFED))
...
```

* (fictive) example for ictce toolchain:

```
[DEPENDENCIES]
icc(2013_sp1.1)
ifort(2013_sp1.1)
imkl(2013_sp1.1)
impi(2013_sp1.1)
```

* exceptions for dependencies, based on toolchain

```
[goolf == 1.1.10]
[[DEPENDENCIES]]
CMake(2.8.5, gompi == 1.1.10)
```

* sanity check section:

```
[SANITY_CHECK]
files = foo, bar
dirs = bin, lib
```

* work out Scalasca example

```
# EASYBUILDFORMAT 2.0

name = 'Scalasca'

[SUPPORTED]
versions = > 2.0
toolchains = goolf == 1.4.10, ictce == 5.5.0, goalf == 1.1.0 suffix:-no-OFED

[DEPENDENCIES]
Score-P = >= 1.2
Cube = > 4.0  # Cube/4.0-goolf-1.4.10, Cube/4.0-ictce-5.5.0
Opari = > 1.0, dummy == dummy  # Opari/1.0
[[goolf]]
zlib = 1.2.8, gompi == 1.4.10  # zlib/1.2.8-gompi-1.4.10
[[goalf]]
zlib = 1.2.8, gompi > 1.0
[[ictce]]
zlib = (> 1.0 && < 3.0) || != 2.4.5   # version range example

# versionsuffix section
[> 0.0 suffix:-foo]
versionsuffix = -foo
```


## Discussions

* Cube really shouldn't sneakily add `--disable-gui` if it doesn't like the Qt version it finds

* stacked variables support in Lmod for defining e.g. `$CC`, etc.
 * still leaves issues with unloading modules and surprising users (unless the modules tool is very verbose w.r.t. redefining env vars)
 * Markus: modules shouldn't define `$CC`, etc. 
 * also ties to `mpicc` and its behavior (e.g. default compiler is GCC for Intel MPI's `mpicc`)


### Hackathon 

* Inge: make Elpa build work after setting `$FCFLAGS` and `$LIBS`, providing patch file for tests, set parallel = 1, etc.

* **TODO**: fix website: throw out disclaimer, fix demo, mention bootstrap as primary way of installing it

* discussions on toolchain-independent modules
 * build with goolf or dummy, statically link, generate module with dummy toolchain
 * see https://github.com/hpcugent/easybuild-framework/issues/570

* Fotis: full bootstrap:
 * build Python, Lua, Lmod
 * bootstrap EasyBuild on top
 * install Python/Lua/Lmod modules using `ev`
 * source a script to set up Lmod environment
 * install EasyBuild with EasyBuild :)

* Markus: interest in adding support for building/using other compilers: Open64, Path64, PGI, …
 * TODO: send old easyblocks for Open64 + Path64 to Marcus (**update**: done)

* Fotis: pymodules, modules tool implemented in Python
 * Robert McLay developed a Python modules tool in a previous life

* Alan: set up Lmod on JUROPA3
 * use Lmod provided by EasyBuild
 * create extra module to use as dependency to avoid requiring `source $EBROOTLMOD/init/bash`

* Gunther: build own Intel toolchain (`intel/14.0.1`) with customizations to Intel MKL (e.g. MKLPATH), ...


### Next steps post-hackathon

* Markus:
 * hierarchical modules
 * support more compilers (building and using them)
 * add support for job submission to SLURM
 * ...
* Bernd + Anke: switch from UNITE to EB, add support for new (versions of) performance tools, …
     write easyblock for Tau, HPC Toolkit, ...
     replace current software packaging crap with EasyBuild?
* Alan: pitch use of EasyBuild and Lmod on JUROPA3
 * switch to Lmod may be sensitive, to make sure it works everywhere w.r.t. setting up Lmod environment (e.g. make sure tclsh works everywhere)
* HPC-UGent:
 * fix issues uncovered during hackathon
 * documentation on getting started with easyblocks, e.g. how do you replace easyconfig parameters with sections in an easyblock, concrete use cases, examples, ...
 * look into JuBe in a VSC context

### JUBE
 * rewrite from scratch after studying the design and features?
 * EasyBuild resolves a large part of it
 * current framework in Perl is too broken to further extend it
 * can be used for testing EasyBuild, UNITE performance tools, ...
 * `mympirun` (and `myscoop`) can fill in a a large chunk of this too?

### another idea for dependency specifications

(by Kenneth, after hackathon)

```
dependency_1-gcc = (GCC, 4.7.2)
dependency_2-openmpi = (OpenMPI, 1.6.4, GCC == 4.7.2)
dependency_3-openblas = (OpenBLAS, >= 0.2.6, suffix:-LAPACK-3.4.2, gompi == 1.1.10 suffix:-no-OFED)
dependency_4-fftw = (FFTW, 3.3.3, gompi == 1.1.10 suffix:-no-OFED)
dependency_5-scalapack = (ScaLAPACK, 2.0.2, suffix:-OpenBLAS-0.2.6-LAPACK-3.4.2, gompi == 1.1.10 suffix:-no-OFED)
```

```
dependency_1 = GCC(4.7.2)
dependency_2 = OpenMPI(1.6.4, GCC == 4.7.2)
dependency_3 = OpenBLAS(>= 0.2.6, suffix:-LAPACK-3.4.2, gompi == 1.1.10 suffix:-no-OFED)
dependency_4 = (FFTW, 3.3.3, gompi == 1.4.10)
dependency_5 = (ScaLAPACK, 2.0.2, suffix:-OpenBLAS-0.2.6-LAPACK-3.4.2, gompi == 1.1.10 suffix:-no-OFED)
```