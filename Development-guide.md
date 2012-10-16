So you want to extend EasyBuild? Great idea!

There are three major parts to *EasyBuild*:
 * build.py: the main script which sets up the rest of the application and determines the build-order, this is called through easybuild.sh which sets a correct Python environment before starting
 * apps: the general Application class and a collection of more specific subclasses
 * buildsoft: utilities for building
   * asyncprocess
   * buildLog: wrapper around Python's logger
   * config: proxy for the [[Configuration|Configuration]]
   * fileTools: file management and running commands
   * moduleGenerator: generates module-files
   * modules: interface to the module command
   * repository: interface to the configured source control system such as [[pySVN|http://pysvn.tigris.org/]] subversion or the filesystem.
   * toolkit: toolkit setup and configuration

## How to build a custom Application-class

First check if the available functionality is already available in EasyBuild. Some application-classes are very flexible and might be sufficient for your needs.
If not, go ahead and create a new class inheriting from Application or a subclass of that (you can get a quick overview of all available classes using `--dump-classes`).

Each step in the build-process consists out of one or more methods which you can override. If you want to skip a step, just implement it with pass. See [[Easyconfig files|Easyconfig-files]] for options for each step in the default implementation.

 1. Generate installation path and create build directory
     * `genInstallDir`
     * `makeBuilDir`
 1. Unpack source in the build directory
     * `unpackSrc`
 1. Apply patches
     * `applyPatch`
 1. Configure
     * prepare toolkit (using self.tk.prepare)
     * `startFrom`: Change the build directory
     * `configure`
 1. Make
     * `make`
 1. Test (run make test or variants)
     * `test`
 1. Install
    * `makeInstallDir`
    * `makeInstall`
 1. Packages
    * `packages`
 1. Generate modulefiles
    * `postproc`
 1. Cleanup
    * `cleanup`
 1. Test installation
    * `runTests`

## Adding specification-options

Specification files are read by the `importCfg`-method. Instead of overriding this method, it's highly recommended to extend the datastructure that method uses to process your specification options.
To do so, create the following extra_options method:
```python
from easybuild.framework.easyconfig import CUSTOM
from easybuild.easyblocks.application import Application

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

