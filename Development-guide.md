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

## Adding specification-options

Specification files are read by the `importCfg`-method. Instead of overriding this method, it's highly recommended to extend the datastructure that method uses to process your specification options.
To do so, create the following extra_options method:
```python
from easybuild.framework.easyconfig import CUSTOM
from easybuild.easyblocks.generic import ConfigureMakeInstall


    def __init__(self, *args, **kwargs):
        Application.__init__(self, *args, **kwargs)

    @staticmethod
    def extra_options():
        extra_vars = [

                      ('importdeps', [None, 'A list of modules to import when configuring', CUSTOM]),
                      ('config-key', [<default>, <description>, CUSTOM ]),
                      (..., [..., ..., CUSTOM]),
                     ]
        return Application.extra_options(extra_vars)
```
Afterwards you can read the requested configuration using `getCfg`. If a key was not set, the default value will be returned.

## Testing

Unittests should be added (if possible) for each added feature. You can run the unittests with `python -m easybuild.test.suite` 

Adding more unittests should be done by creating a new test module, adding a suite() method which returns a TestSuite object with all the testcases in it.

In easybuild/test/suite.py you should add it to the list of modules then, so it will be included when somebody runs the suite.

## Regression test

To run a full regression test: first append the easybuild directory to the PYTHONPATH. Also, set your MODULEPATH to something which doesn't have dependencies installed.
then run `python easybuild/scripts/regtest.py`. (see -h) for specific options.

output will be placed in the current directory in easybuild-test-TIMESTAMP. You can aggregate the results into a single xml file. by using the -a option.

Make sure to populate the easybuild_config.py with settings that make sense for the regression tester.

The final xml will contain JUnit-compatible xml. Per build there is either a failure (with reason).
the buildstats for each application and also a summary in a top comment. 

