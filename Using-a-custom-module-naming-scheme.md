**[2013911] Note: this feature is currently in development, and thus this documentation is preliminary.**

Since EasyBuild v1.8.0, you can use a self-defined alternative module naming scheme, instead of the default EasyBuild module naming scheme. This page describes how to implement such a custom module naming scheme, and how to direct EasyBuild to make use of it.

### Implementing a custom module naming scheme

#### Step 0: Preparation: extend the `module_naming_scheme` namespace

To implement a custom module naming scheme for EasyBuild, you must provide a class that derives from the 'abstract' class `ModuleNamingScheme` and implements a class method named `det_full_module_name`, in a module in the `easybuild.tools.module_naming_scheme` namespace.

This can be done as follows:

```bash
# create paths
EB_MNS_DIR=$HOME/easybuild/tools/module_naming_scheme
mkdir -p $EB_MNS_DIR
# make Python packages span across multiple directories
cat << EOF > $EB_MNS_DIR/__init__.py  # easybuild.tools.module_naming_scheme namespace
from pkgutil import extend_path
# we're not the only ones in this namespace
__path__ = extend_path(__path__, __name__)
EOF
cp $EB_MNS_DIR/__init__.py $EB_MNS_DIR/../__init__.py  # easybuild.tools namespace
cp $EB_MNS_DIR/__init__.py $EB_MNS_DIR/../../__init__.py  # easybuild namespace
# create empty example_module_naming_scheme Python module
touch $EB_MNS_DIR/example_module_naming_scheme.py
```

Note that the particular name of the Python module file (`example_module_naming_scheme.py` in this example) does not matter.

#### Step 1: Implement your custom module naming scheme

In module you've just created, the `det_full_module_name` method should be implemented that receives a dictionary-like value as argument which represents a parsed easyconfig file, and need to produce the module name as a string.
The class providing this function must derive from `ModuleNamingScheme` which is provided by the EasyBuild framework.

Below, we implement a simple module naming scheme that uses the toolchain name and version as prefix in the module name to yield a hierarchically organized set of modules, e.g. `ictce/4.1.13/gzip/1.5`, `goolf/1.4.10/gzip/1.5`, etc., rather than as an extension to the version (as is done in the default EasyBuild module naming scheme, e.g. `gzip/1.5-ictce-4.1.13`):

```python
import os

from easybuild.tools.module_naming_scheme import ModuleNamingScheme


class ExampleModuleNamingScheme(ModuleNamingScheme):
    """Class implementing an example module naming scheme."""

    def det_full_module_name(self, ec):
        """
        Determine full module name from given easyconfig, according to an example module naming scheme.

        @param ec: dict-like object with easyconfig parameter values (e.g. 'name', 'version', etc.)

        @return: string representing full module name, e.g.: 'ictce/4.1.13/gzip/1.5'
        """

        import os

        # fetch required values
        name = ec['name']
        version = ec['version']
        tc_name = ec['toolchain']['name']
        tc_version = ec['toolchain']['version']

        # compose module name by stitching parts together, toolchain first
        return os.path.join(tc_name, tc_version, name, version)
```

#### Step 2: Making EasyBuild use the custom module naming scheme

To make EasyBuild use our custom module naming scheme, we need to make sure the path where it is located, i.e. the path where we created `easybuild/tools/module_naming_scheme/example_module_naming_scheme.py`, is included in `$PYTHONPATH`.

```bash
export PYTHONPATH=$PYTHONPATH:$HOME
```

Via the EasyBuild configuration option `--module-naming-scheme` (or, equivalently, the environment variable `$EASYBUILD_MODULE_NAMING_SCHEME`), you need to specify our custom module naming scheme should be used (using the class name of our module naming scheme implementation):

```bash
eb --module-naming-scheme=ExampleModuleNamingScheme test.eb --robot --dry-run
# or
export EASYBUILD_MODULE_NAMING_SCHEME=ExampleModuleNamingScheme
eb test.eb --robot --dry-run
```

#### Step 3: Test your custom module naming scheme.

To test the implementation of your custom module naming scheme, and whether EasyBuild picks it up, try the following:

```bash
$ eb --avail-module-naming-schemes
List of supported module naming schemes:
	EasyBuildModuleNamingScheme
	ExampleModuleNamingScheme
```

```bash
$ eb --module-naming-scheme=ExampleModuleNamingScheme gzip-1.5-goolf-1.4.10.eb --robot --dry-run | sed 's@ /.*easyconfigs@@g'
== temporary log file in case of crash /var/folders/6y/x4gmwgjn5qz63b7ftg4j_40m0000gn/T/easybuild-A9554O.log
== Dry run: printing build status of easyconfigs and dependencies
[ ]/g/GCC/GCC-4.7.2.eb (module: dummy/dummy/GCC/4.7.2)
[ ]/h/hwloc/hwloc-1.6.2-GCC-4.7.2.eb (module: GCC/4.7.2/hwloc/1.6.2)
[ ]/o/OpenMPI/OpenMPI-1.6.4-GCC-4.7.2.eb (module: GCC/4.7.2/OpenMPI/1.6.4)
[ ]/g/gompi/gompi-1.4.10.eb (module: dummy/dummy/gompi/1.4.10)
[ ]/o/OpenBLAS/OpenBLAS-0.2.6-gompi-1.4.10-LAPACK-3.4.2.eb (module: gompi/1.4.10/OpenBLAS/0.2.6)
[ ]/f/FFTW/FFTW-3.3.3-gompi-1.4.10.eb (module: gompi/1.4.10/FFTW/3.3.3)
[ ]/s/ScaLAPACK/ScaLAPACK-2.0.2-gompi-1.4.10-OpenBLAS-0.2.6-LAPACK-3.4.2.eb (module: gompi/1.4.10/ScaLAPACK/2.0.2)
[ ]/g/goolf/goolf-1.4.10.eb (module: dummy/dummy/goolf/1.4.10)
[ ]/g/gzip/gzip-1.5-goolf-1.4.10.eb (module: goolf/1.4.10/gzip/1.5)
```

Note the `(module: <string>)` part in the output that indicates that our custom module naming scheme is used, as opposed to the default EasyBuild module naming scheme (which would yield `gzip/1.5-ictce-1.4.10` for example).

### Attention points

There are a couple of important attention points that should be taken into account when designing a custom module naming scheme:

 * make sure the **module naming scheme yields unique modules**, such that there a no false positives when EasyBuild checks whether a module is already available
  * this boils down to always taking into account at least the following easyconfig parameters: `name`, `version`, `toolchain` (both `name` and `version`), and `versionsuffix`

 * make sure **no clashes between module files and module path subdirectories** can occur
  * e.g., a module naming scheme that consistently uses the toolchain `<name>/<version>` as a prefix would not work, because for example first a module file in the path `goolf/1.4.10` would be created, and then a path like `goolf/1.4.10/gzip/1.5` would be required but impossible since `1.4.10` can't be both a file and a directory at the same time...


### Current limitations

Currently, only the `name`, `version`, `versionsuffix` and `toolchain` easyconfig parameters are available in the dictionary(-like) value that is passed to the `det_full_module_name` function. See [easybuild-framework issue #687](https://github.com/hpcugent/easybuild-framework/issues/687) for more information.