The process for building a package is fully configured through the use of easyconfig files. These files contain all configuration to perform a successful build.
EasyBuild-files are interpreted as Python-code, so they can contain some dynamic configuration as well. Note that this also means that these easyconfigs are sensitive to indentation and whitespace.

Some settings are required and must be present:

 * **name**: Name of software
 * **version**: Version of software
 * **homepage**: The homepage of the software
 * **description**: A short description of the software
 * **toolchain**: See below

Possible configuration options can also be retrieved by running `eb` with the -o or --options switch. Options for a specific application class can be found by adding a -m option.

Another very important option is **easyblock**. This defines the EasyBlock (Python-class subclassing Application) that controls the build-process and overrides standard behavior for specific cases.

If you want to make use of features only recently introduced in EasyBuild, it is highly recommended that you set the **easybuildVersion** setting. Older versions of EasyBuild will then refuse to build this easyconfig.

## Toolchain

EasyBuild uses the concept of a toolchain to define the compiler and a set of libraries that will be used during the build of the application.

The following toolchains are currently defined:

 * **dummy**: no compiler toolchain
 * **gcc**: GNU Compiler Collection
 * **gmgfl**: GCC + LAPACK + GotoBLAS + FLAME + MVAPICH2
 * **gimkl**: GCC + IMPI + IMKL
 * **gqacml**: GCC + QLogic MPI + ACML
 * **ictce**: Intel Cluster Toolkit Compiler Edition
 * **iqacml**: ICC + QLogic MPI + ACML

A toolchain is selected by configuring it as follows in the easyconfig:
`toolchain = {'name':'ictce', 'version':'4.0.4'}`

If you do not require a toolchain (e.g. for a binary or interpreted package), use a dummy toolchain.
` toolchain = {'name': 'dummy', 'version': ''} `

If you do **not** want to load the dependencies of the module when using a dummy toolchain, also specify the toolchain version as dummy:
` toolchain = {'name': 'dummy', 'version':'dummy'} `

The toolchain name and version will automatically be added to the version-string identifying the built package. This way it is possible to install the same version of a package using different toolchains.

 * **toolchain**: dictionary with 2 keys, _name_: name of the toolchain and _version_: version of the toolchain
 * **toolchainopts**: dictionary with extra options for the compiler. Supported options include:
  * _usempi_: boolean to indicate if MPI should be used (default: False)
  * _cciscxx_: boolean to indicate whether the environment variable `CXX` should be set to the same as `CC` (default: False)
  * _pic_: boolean to indicate whether -fPIC should be used (default: False)
  * _opt_: boolean to indicate whether heavy optimization should be performed (i.e. use `-O3`) (default: False)
  * _noopt_: boolean to indicate whether optimization should be disabled (i.e. use `-O0`) (default: False)
  * _lowopt_: boolean to indicate whether low optimization should be performend (i.e. use `-O1`) (default: False)
  * _debug_: boolean to indicate whether the `-g` option should be used (default: False)
  * _optarch_: boolean to indicate whether architecture specific optimization should be done (i.e. use `-march=native` (GCC), `-xHOST` (icc @ Intel) or `-msse3` (icc @ AMD)) (default: True)
  * _i8_: boolean to indicate whether default integer size should be set to 8 (i.e. use `-i8` (icc) or `-fdefault-integer-8` (GCC)) (default: False)
  * _unroll_: boolean to indicate whether loop unrolling should be performed (i.e. use `-unroll))) (icc) or `-funroll-loops` (GCC)) (default: False)
  * _verbose_: boolean to indicate whether verbose output should be enabled (i..e use `-v`) (default: False)
  * _cstd_: string to specify which C standard should be used, by using `-std=<string>` (default: None)
  * _shared_: boolean to indicate whether `-shared` should be used during linking (default: False)
  * _static_: boolean to indicate whether `-static` should be used during linking (default: False)
  * _intel-static_: boolean to indicate whether `-intel-static` should be used during linking (only for Intel compilers) (default: False)
  * _loop_: boolean to indicate whether advanced loop optimizations should be enabled (default: False)
  * _f2c_: boolean to indicate whether `-ff2c` should be used (only GCC) (default: False)
  * _no-icc_: boolean to indicate whether `-no-icc` should be used during linking (only for Intel compilers) (default: False)
 * **onlytkmod**: boolean/string to indicate if the toolchain should only load the environment with module (True) or also set all other variables (False) like compiler CC etc (If string: comma separated list of variables that will be ignored). (Default: False)

## Blocks

Another feature of easybuild-files are blocks. Using blocks you can easily build multiple varieties of the same software at the same time. Easyconfigs using blocks consist out of a common part and one or more blocks.
Variants can also depend on each other, by setting the **block**-option inside the block. When using source-control, the different blocks as well as the original block file will be committed.

Example:

> mod="Compiler.Gcc"

> name="GCC"

> homepage='http://gcc.gnu.org/'
> description="The GNU Compiler Collection includes front ends for C, C++, Objective-C, Fortran, Java,
> and Ada, as well as libraries for these languages (libstdc++, libgcj,...)."

> [variant-a]
>
>\# Extra settings here

> [variant-b]
>
>\# An extension of variant-a
>
> block = "variant-a"


## Build options
The standard build process consists out of the following steps. Configuration options interacting with each step are shown inline.

 1. Generate installation path and create build directory
 1. Unpack source in the build directory
     * **sources**: List of source-items, a source item can be either a string with the name of the source package, or a tuple of (name of source package, command to unpack this source package)
     * **source_urls**: List of URLS where to find the packages source code, so it can automatically be downloaded.
     * **unpackOptions**: Extra options for unpacking source (default: None)
 1. Apply patches
     * **patches**: List of patches to apply.

     There are three ways to specify a patch: a patch file with name indicated by a string (e.g. "`fix.patch`"), a patch file with a specified patch level with a list of two elements (name and patch level, e.g. `["fix.patch",2]`), or a file to copy with a file name and path suffix indicated by a list of two elements (e.g. `["myfile.txt","path/to/where/file/should/be"]`).
 1. Configure
     * **preconfigopts**: Extra options pre-passed to configure.
     * **configopts**: Extra options passed to configure (Default already has --prefix)
 1. Make
     * **premakeopts**: Extra options pre-passed to make.
     * **makeopts**: Extra options passed to make (Default already has -j X)
     * **parallel**: The number of build threads (make -j X) than can be used. (Default: based on the number of cores and restrictions in ulimit)
 1. Test (run make test or variants)
     * **runtest**: Indicates if a test should be run after make, value will be passed as `make [runtest]`.
 1. Install
    * **installopts**: Extra options for installation (Default: nothing)
 1. Packages: see Package options
 1. Perform a sanity check, check whether some directories and files are present after installation
    * **sanityCheckPaths**: List of files and directories to check (format: ` {'files':<list>, 'dirs':<list>} `, default: ` { } `)
    * **sanityCheckCommand**: Command to run, should exit with code 0 (format: `(name, options) `, default: ` None `).
      If you just specify `True`, easybuild will run `name -h`. You can specify
      a command using a tuple. If you leave a value to `None` easybuild will use
      the default (the module name and -h as option)
 1. Generate modulefiles
    * **modextravars**: Extra environment variables to be added to module file (default: `{ }`)
 1. Test installation
    * **tests**: List of test-scripts to run after install. A test script should return a non-zero exit status to fail

It is possible to customize the version-string used in this process, for example to distinguish multiple variants of a package.

 * **versionprefix**: Additional prefix for software version
 * **versionsuffix**: Additional suffix for software version
The final version-string consists out of the following components (if given): ` [versionprefix][version]-[toolchain]-[toolchainversion][versionsuffix] `

## File management options

 * **startfrom**: Path to start the make in. If the path is absolute, use that path. If not, this is added to the guessed path.
 * **cleanupoldbuild**: Boolean to remove (True) or backup (False) the previous build directory with identical name or not. Default True
 * **cleanupoldinstall**: Boolean to remove (True) or backup (False) the previous install directory with identical name or not. Default True
 * **dontcreateinstalldir**: Boolean to create (False) or not create (True) the install directory (Default False)
 * **keeppreviousinstall**: Boolean to keep the previous installation with identical name. (Default False) **Experts only! **
 * **keepsymlinks**: Boolean to determine whether symlinks are to be kept during copying or if the content of the files pointed to should be copied (Default: False)

## Dependencies

Software can depend on one or more modules which will be automatically loaded with the modules-system. After building these modules will be exported to the generated modulefile.
Optionally you can also specify a list of builddependencies, which will be loaded at build-time but not exported afterwards.

 * **dependencies**: List of dependencies, i.e. modules that should also be load when the this package is loaded (default: [])
 * **builddependencies**: List of build dependencies, i.e. modules that should only be loaded to build this package (default: [])
 * **osdependencies**: List of OS packages that should be present on the system (verified with RPM) (default: [])

Dependencies and builddependencies should be in the following format: ` (name, version [, versionsuffix [, True to use a dummy-toolchain ]]) `

Example:

  CP2K-20110124-gimkl-0.5.1.eb contains the following toolchain and dependencies

```python
toolchain = {'name': 'gimkl', 'version': '0.5.1'}
dependencies = [('Libint', '1.1.4')]
```

  When installing with this easyconfig, the build system will look for dependency with name 'Libint' and version '1.1.4-gimkl-0.5.1'.

To include dependencies that are independent of the toolchain, i.e. built with a `dummy` toolchain, you should use a 4-tuple with the last element set to `True`, e.g.:

```python
dependencies = [('Java', '1.7.0_21', '', True)]
```

## License options

 * **group**: Name of the user group for which the software should be available
 * **key**: Key for installing software
 * **licenseServer**: License server for software
 * **licenseServerPort**: Port for license server

## Extension options

For software like Perl, Python, Ruby, R, Octave and Tcl extension packages can be installed (optionally).

The following easyconfig parameter provide the necessary support for specifying which extensions should be installed.

 * **exts_list**: List with extensions added to the base installation (default: []). See below for details.
 * **exts_defaultclass**: List of module for and name of the default extension class (default: None)
 * **exts_filter**: Extension filter details. List with template for cmd and input to cmd (templates for name, version and src). (default: None)

Extensions should be specified via the **exts_list** parameter in one of the following three ways:
 * just the name, as a string, e.g.: ```exts_list = ['numpy']```; this will works for extensions for which the latest available version can be automatically determined (e.g. for R)
 * name and version, as a tuple or list of strings, e.g.: ```exts_list = [('numpy', '1.6.1'), ['scipy', '0.10.1']]```
 * name, version and options, with options specified as a dictionary: ```exts_list = [('numpy', '1.6.1', {'source_tmpl': '%(name)-%(version).tar.bz2', 'patches': ['one.patch', 'two.patch']})]```

The following options can be set per extension via the 3rd item of an element of **exts_list**:
 * **modulename**: module name for extension, useful when it derives from extension name (e.g., ```setuptools``` for ```distribute``` Python package)
 * **nosource**: boolean to indicate that no source file should be search for
 * **patches**: list of patch files for this extension
 * **source_urls**: download URLs for source file
 * **source_tmpl**: template for name of source file (default used is ```%(name)s-%(version)s.tar.gz```

## Package options
**OBSOLOTE, SHOULD BE REMOVED (see Extension options above)**
Software like Perl, Python, Ruby, R, Octave and Tcl can optionally install addon packages.

 * **pkglist**: List with packages added to the baseinstallation (Default: []). Each package should be a list or tuple consisting out of a name and version.
 * **pkgcfgs**: Dictionary with config parameters for packages (default: {})
 * **pkgdefaultclass**: List of module for and name of the default package class (Default: None)
 * **pkgfilter**: Package filter details. Tuple with template for cmd and input to cmd (templates for name, version and src). (Default: None)
                  Will be run after installation. Can be compared to the
                  sanityCheckCommand.
 * **pkgfindsource**: Find sources for packages (Default: True)
 * **pkginstalldeps**: Install dependencies for specified packages if necessary (Default: True)
 * **pkgloadmodule**: Load the to-be installed software using temporary module (Default: True)
 * **pkgmodulenames**: Dictionary with real modules names for packages, if they are different from the package name (Default: {})
 * **pkgpatches**: List with patches for packages (default: []). This should be a list consisting out [_package-name_, [_patch-file_, ...]] entries
 * **pkgtemplate**: Template for package source file names (Default: %s-%s.tar.gz)
 * **requirements**: Packages required by this module

(Hint: if you want to install extra packages for software that is already installed, modify the easyconfig and use EasyBuild with the --skip and --force options.)

## Build statistics
After each successfull build and installation a couple of build statistics get saved in the .eb file.
buildstats is a list of dictionaries. Each dictionary containing:

* timestamp: a unix timestamp presenting the time this build was finished.
* cpu_model: a string representing the cpu this build was made on. e.g., Intel(R) Core(TM) i5-2540M CPU @ 2.60GHz
* core_count: the amount of cores in this system.
* build_time: the amount of seconds it took to build and install this package.
* platform: information about the platform this build was made (as returned by platform.platform(): e.g., Linux-3.3.1-3.fc16.x86_64-x86_64-with-fedora-16-Verne
host: the hostname of the machine this build was made on.
* install_size: the number of bytes in the installdir after installation.

## Software package specific options

You can define your own additional software specific entires in the easyconfig, which can then be used in the Python class that supports the build process for that package.
See the 'Adding pecification-options' section in[the easyblock development guide](https://github.com/hpcugent/easybuild/wiki/Development-guide)

## sanity Check
The `sanity_check_paths` dict defines a list of files and directories that should be present after a successful installation. If EasyBuild can not find them the installation will be considered failed.
This defaults to a `{'files': [], 'dirs': ['lib', 'bin']}
```python
sanity_check_paths = {
    'files': ['bin/gpaw%s' % x for x in ['', '-basis', '-mpisim', '-python', '-setup', '-test']],
    'dirs': ['lib/python%s/site-packages/%s' % (pythonshortver, name.lower())]
}
```