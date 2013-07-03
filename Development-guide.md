So you want to extend EasyBuild? Great idea!

There are three major parts to *EasyBuild*:
 * [Framework](https://github.com/hpcugent/easybuild-framework) with:
   * main.py: the main script which sets up the rest of the application and determines the build-order, this is called through the `eb` command which sets a correct Python environment before starting.
   * framework: the general EasyBlock, EasyConfig and Extension classes.
   * tools: General utilities used trought the framework code.
     * asyncprocess: Written by  Josiah Carlson (http://code.activestate.com/recipes/440554/ )
     * build_log: wrapper around Python's logger (will be replaced by fancylooger soon
     * config: proxy for the [[Configuration|Configuration]]
     * filetools: file management and running commands
     * module_generator: generates module-files
     * modules: interface to the module command
     * repository: interface to the configured source control system such as [[pySVN|http://pysvn.tigris.org/]] subversion or the filesystem.
     * toolchains: Contains implementations of toolchain components with their switches and flags. This inculdes the compiler, math libs, mpi implementations...
     * others...

 * [EasyBlocks](https://github.com/hpcugent/easybuild-easyblocks): Custom EasyBlock classes are grouped here.

 * [EasyConfig](https://github.com/hpcugent/easybuild-easyconfigs): A (huge) list of example EasyConfigs

## How to build a custom EasyBlock-class

First check if the available functionality is already available in EasyBuild. Some [Generic EasyBlock classes](https://github.com/hpcugent/easybuild-easyblocks/tree/master/easybuild/easyblocks/generic) are very flexible and might be sufficient for your needs.
If not, go ahead and create a new class inheriting from EasyBlock or a Generic subclass or even an specific software package class (you can get a quick overview of all available classes using `--dump-classes`).

Each step in the build-process consists out of one or more methods which you can override. If you want to skip a step, just implement it with pass. See [[Easyconfig files|Easyconfig-files]] for options for each step in the default implementation.

 1. Get the sources:
     * `fetch_step`: fetches files, either from local filesystem, or from a remote url. (We only support http and ftp for now, git/svn support [is on the wishlist](https://github.com/hpcugent/easybuild-framework/issues/112), contact us if you feel like adding it)
 1. Generate installation path and create build directory
     * `check_readinesstep`
     * `gen_installdir`
     * `make_builddir`
     * `reset_changes`: resets changes in the environment
 1. Unpack source in the build directory
     * `checksum_step` : currently empty, see https://github.com/hpcugent/easybuild-framework/issues/214
     * `extract_step`
 1. Apply patches
     * `patch_step`
 1. Configure
     * `prepare_step`: prepare toolkit (using self.tk.prepare)
     * `configure_step`
 1. Build
     * `build_step`
 1. Test (run make test or variants)
     * `test_step`
 1. Install
    * `stage_install_step`
    * `make_installdir`
    * `install_step`
 1. Extensions: build extensions (if any)
    * `extensions_step`
 1. Upload eb files to repository
    * `post_instal_step`
 1. Check installed files
    * `sanity_check_step`
 1. Cleanup
    * `cleanup_step`
 1. Create module
    * `make_module_step`

## Specification-options
Some specification options are added in an EasyConfig, to see the available options, run `eb --avail-easyconfig-params`
On EasyBlock this returns:
```
MANDATORY
---------
name:			Name of software
version:		Version of software
toolchain:		Name and version of toolchain
description:		A short description of the software
homepage:		The homepage of the software

EASYBLOCK-SPECIFIC
------------------
tar_config_opts:	Override tar settings as determined by configure.

TOOLCHAIN
---------
toolchainopts:		Extra options for compilers
onlytcmod:		Boolean/string to indicate if the toolchain should only load the environment with module (True) or also set all other variables (False) like compiler CC etc (if string: comma separated list of variables that will be ignored). (default: False)

BUILD
-----
easybuild_version:	EasyBuild-version this spec-file was written for
versionsuffix:		Additional suffix for software version (placed after toolchain name)
versionprefix:		Additional prefix for software version (placed before version and toolchain name)
runtest:		Indicates if a test should be run after make; should specify argument after make (for e.g.,"test" for make test) (default: None)
preconfigopts:		Extra options pre-passed to configure.
configopts:		Extra options passed to configure (default already has --prefix)
premakeopts:		Extra options pre-passed to build command.
makeopts:		Extra options passed to make (default already has -j X)
preinstallopts:		Extra prefix options for installation (default: nothing)
installopts:		Extra options for installation (default: nothing)
unpack_options:		Extra options for unpacking source (default: None)
stop:			Keyword to halt the buildprocess at certain points. Valid are ['cfg', 'source', 'patch', 'prepare', 'configure', 'make', 'install', 'test', 'postproc', 'cleanup', 'extensions']
skip:			Skip existing software (default: False)
parallel:		Degree of parallelism for e.g. make (default: based on the number of cores and restrictions in ulimit)
maxparallel:		Max degree of parallelism (default: None)
sources:		List of source files
source_urls:		List of URLs for source files
patches:		List of patches to apply
tests:			List of test-scripts to run after install. A test script should return a non-zero exit status to fail
sanity_check_paths:	List of files and directories to check (format: {'files':<list>, 'dirs':<list>}, default: {})
sanity_check_commands:	format: [(name, options)] e.g. [('gzip','-h')]. Using a non-tuple is equivalent to (name, '-h')

FILE-MANAGEMENT
---------------
start_dir:		Path to start the make in. If the path is absolute, use that path. If not, this is added to the guessed path.
keeppreviousinstall:	Boolean to keep the previous installation with identical name. (default: False) Experts only!
cleanupoldbuild:	Boolean to remove (True) or backup (False) the previous build directory with identical name or not. (default: True)
cleanupoldinstall:	Boolean to remove (True) or backup (False) the previous install directory with identical name or not. (default: True)
dontcreateinstalldir:	Boolean to create (False) or not create (True) the install directory (default: False)
keepsymlinks:		Boolean to determine whether symlinks are to be kept during copying or if the content of the files pointed to should be copied

DEPENDENCIES
------------
dependencies:		List of dependencies (default: [])
builddependencies:	List of build dependencies (default: [])
osdependencies:		OS dependencies that should be present on the system

LICENSE
-------
license_server:		License server for software
license_serverPort:	Port for license server
key:			Key for installing software
group:			Name of the user group for which the software should be available

EXTENSIONS
----------
exts_list:		List with extensions added to the base installation (default: [])
exts_defaultclass:	List of module for and name of the default extension class (default: None)
exts_filter:		Extension filter details: template for cmd and input to cmd (templates for name, version and src). (default: None)

MODULES
-------
modextravars:		Extra environment variables to be added to module file (default: {})
moduleclass:		Module class to be used for this software (default: base) (valid: ['base', 'compiler', 'lib'])
moduleforceunload:	Force unload of all modules when loading the extension (default: False)
moduleloadnoconflict:	Don't check for conflicts, unload other versions instead (default: False)

OTHER
-----
buildstats:		A list of dicts with build statistics

```

## Adding specification-options
Sometimes you want to add extra options that should be configurable in the EasyConfig file.

EasyConfig files are read by the `EasyConfig`-class. Instead of overriding this class, we extend the `extra_options` function in EasyBlock to return list of your specification options.
To do so, create the an extra_options which returns a list of new options that can be added.

## Setting environment variables
Some software can only be configured by setting some environment variables. Use the `easybuild.tools.environment` module for this.
This will set environment variables, and keep track of them in between steps, so you have more information what happened when whilst debugging.

For defining what environment variables to set to what path in the produced environment modules file you need to implement the `make_module_req_guess` method. This method will check if a path exists, and if so create an environment variable pointing to it.
This method should return a dictionary of `{'VARIABLE_NAME': 'directory'}` Default: 
```python
{
    'PATH': ['bin'],                                                                                             
    'LD_LIBRARY_PATH': ['lib', 'lib64'],                                                                         
    'CPATH':['include'],                                                                                         
    'MANPATH': ['man', 'share/man'],                                                                             
    'PKG_CONFIG_PATH' : ['lib/pkgconfig', 'share/pkgconfig'],                                                    
}
```

If you want to define environment variables not related to a path you will have to implement the `make_module_extra` method. This method needs to return a string to add to the module file after the automatic dependencies and paths are set.

## Example EasyBlock

```python
from easybuild.framework.easyconfig import CUSTOM
from easybuild.easyblocks.generic import ConfigureMake
from easybuild.tools import environment


class EB_MySoftware(ConfigureMake)
    """Install MySoftware"""

    def __init__(self, *args, **kwargs):
        """Constructor"""
        ConfigureMake.__init__(self, *args, **kwargs)
        self.subdir = ""

    def configure_step(self):                                                                                            
        """Configuration step, we set FC to F90,
        F77 and F90 are already set by EasyBuild to the right compiler, but this tool uses FF for F90 compiler.
        """ 
        environment.setvar("FC", self.toolchain.get_variable('F90'))   
        self.cfg.update('makeopts', CC="%(CC)s"' % {'CC': os.getenv('CC')})                                                  
        ConfigureMake.configure_step(self)
        # different subdir if doing a source install
        if self.cfg['sourceinstall']:
            self.mysubdir = "%s-%s" % (self.name.lower(), self.version)

    @staticmethod
    def extra_options():
        extra_vars = [
            ('sourceinstall', [False, 'Perform an installation in a different subdirectory', CUSTOM]),
            # ('config-key', [<default>, <description>, CUSTOM ]),
        ]
        return ConfigureMake.extra_options(extra_vars)

    def make_module_req_guess(self):                                                                                     
        """Specify correct LD_LIBRARY_PATH and CPATH for this installation."""                                          
        guesses = ConfigureMake.make_module_req_guess(self)                                                          
        guesses.update({                                                                                                 
            'CPATH': [os.path.join(self.subdir, "include")],                                           
            'LD_LIBRARY_PATH': [os.path.join(self.subdir, "lib")]                                      
        })                                                                                               
        return guesses   

    def make_module_extra(self):                                                                                         
        """Set specific environment variables (mysubdir)."""                                                      
        txt = ConfigureMake.make_module_extra(self)                                                                  
        txt += self.moduleGenerator.set_environment('MYSOFTWARE_DIR', '$root/%' % self.mysubdir)
        return txt                                                                                                       
                                                                                                                         
    def sanity_check_step(self):                                                                                         
        """Custom sanity check"""                                                                              
        custom_paths = {                                                                                                 
            'files': [],                                                                                     
            'dirs': [os.path.join(self.mysubdir, x) for x in ["conf", "include", "lib"]]                 
        }                                                                                           
        ConfigureMake.sanity_check_step(self, custom_paths=custom_paths)
```