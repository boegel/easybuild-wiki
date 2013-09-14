**[2013911] Note: this feature is currently in development, and thus this documentation is preliminary.**

Since EasyBuild v1.8.0, you can use a self-defined alternative module naming scheme, instead of the default EasyBuild module naming scheme. This page describes how to implement such a custom module naming scheme, and how to direct EasyBuild to it.

### Implementing a custom module naming scheme

To implement a custom module naming scheme for EasyBuild, you must provide a class that derives from the 'abstract' class `ModuleNamingScheme` and implements a class method named `det_full_module_name`, in a module in the `easybuild.tools.module_naming_scheme` namespace.

This can be done as follows:

```bash
# create paths
mkdir -p $HOME/easybuild/tools/module_naming_scheme
# make Python packages span across multiple directories
cat << EOF > $HOME/easybuild/__init__.py
from pkgutil import extend_path
# we're not the only ones in this namespace
__path__ = extend_path(__path__, __name__)
EOF
cp $HOME/easybuild/__init__.py $HOME/easybuild/tools/__init__.py
cp $HOME/easybuild/__init__.py $HOME/easybuild/tools/module_naming_scheme/__init__.py
# create empty my_module_naming_scheme Python module
touch $HOME/easybuild/tools/module_naming_scheme/my_module_naming_scheme.py
```

Note that the name of the Python module file (`my_module_naming_scheme.py` in this example) does not matter.

In this module, the `det_full_module_name` method should be implemented that receives a dictionary-like value as argument which represents a parsed easyconfig file, and need to produce the module name as a string.
The class providing this function must derive from `ModuleNamingScheme` which is provided by the EasyBuild framework. For example:

```python
import os
from vsc import fancylogger

from easybuild.tools.module_naming_scheme import ModuleNamingScheme


class MyModuleNamingScheme(ModuleNamingScheme):
    """Class implementing a simple module naming scheme for testing purposes."""

    def __init__(self, *args, **kwargs):
        """Initialize logger."""
        self.log = fancylogger.getLogger('my_module_naming_scheme', fname=False)

    def det_full_module_name(self, ec):
        """
        Determine full module name from given easyconfig, according to a simple testing module naming scheme.

        @param ec: dict-like object with easyconfig parameter values (e.g. 'name', 'version', etc.)

        @return: n-element tuple with full module name, e.g.: ('gzip', '1.5'), ('intel', 'intelmpi', 'gzip', '1.5')
        """

        # figure out prefix determined by toolchain
        tc_name = ec['toolchain']['name']
        tc_ver = ec['toolchain']['version']

        # mapping of toolchain name to module prefix
        # alf: ATLAS, (Sca)LAPACK, FFTW
        # olf: OpenBLAS, (Sca)LAPACK, FFTW
        clang_gnu, gnu, intel = 'clang_gnu', 'gnu', 'intel'  # compilers
        intelmpi, openmpi, mpich = 'intelmpi', 'openmpi', 'mpich',  # MPI libs
        mvapich2, qlogicmpi = 'mvapich2', 'qlogicmpi'  # MPI libs (continued)
        alf, acml, olf, mkl = 'alf', 'acml', 'olf', 'mkl'  # math libs
        cuda = 'cuda'  # extras
        tc_prefixes = {
            'ClangGCC': (clang_gnu,),
            'cgmpich': (clang_gnu, mpich),
            'cgmpolf': (clang_gnu, mpich, olf),
            'cgmvapich2': (clang_gnu, mvapich2),
            'cgmvolf': (clang_gnu, mvapich2),
            'cgompi': (clang_gnu, openmpi),
            'cgoolf': (clang_gnu, openmpi, olf),
            'GCC': (gnu,),
            'gmacml': (gnu, mvapich2, acml),
            'gmvapich2': (gnu, mvapich2),
            'gmvolf': (gnu, mvapich2, olf),
            'goalf': (gnu, openmpi, alf),
            'gompi': (gnu, openmpi),
            'goolf': (gnu, openmpi, olf),
            'goolfc': (gnu, openmpi, olf, cuda),
            'iccifort': (intel,),
            'iiqmpi': (intel, qlogicmpi),
            'ictce': (intel, intelmpi, mkl),
            'iomkl': (intel, openmpi, mkl),
            'iqacml': (intel, qlogicmpi, acml),
        }
        if tc_name == 'dummy':
            prefix = ()
        else:
            if tc_name in tc_prefixes:
                prefix = tc_prefixes[tc_name] + (tc_ver,)
            else:
                self.log.error("Don't know how which module prefix to use for toolchain '%s'." % tc_name)

        name_ver = (ec['name'].lower(), ec['version'])

        # add suffix part if there is a suffix
        suff = ec['versionsuffix']
        if suff.startswith('-'):
            suff = suff[1:]
        if suff:
            name_ver = name_ver + (suff, )

        return os.path.sep.join(prefix + name_ver)
```


### Making EasyBuild use the custom module naming scheme

To make EasyBuild use our custom module naming scheme, we need to make sure the path where it is located, i.e. the path where we created `easybuild/tools/module_naming_scheme/my_module_naming_scheme.py`, is included in `$PYTHONPATH`.

```bash
export PYTHONPATH=$PYTHONPATH:$HOME
```

Via the EasyBuild configuration option `--module-naming-scheme` (or, equivalently, the environment variable `$EASYBUILD_MODULE_NAMING_SCHEME`), we need to specify our custom module naming scheme should be used (using the class name of our module naming scheme implementation). For example:

```bash
eb --module-naming-scheme=MyModuleNamingScheme gzip-1.5-goolf-1.4.10.eb --robot --dry-run
```

or

```bash
export EASYBUILD_MODULE_NAMING_SCHEME=MyModuleNamingScheme
eb gzip-1.5-goolf-1.4.10.eb --dry-run
```

### Current limitations

Currently, only the `name`, `version`, `versionsuffix` and `toolchain` easyconfig parameters are available in the dictionary(-like) value that is passed to the `det_full_module_name` function. See [easybuild-framework issue #687](https://github.com/hpcugent/easybuild-framework/issues/687) for more information.