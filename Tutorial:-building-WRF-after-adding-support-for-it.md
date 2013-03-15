This brief tutorial shows how to add support to EasyBuild for building and installation a software package.

For this, we'll use the WRF weather modeling software package, and assume that EasyBuild doesn't already provide the necessary support
to build it (because it does, see the actual [WRF easyblock](https://github.com/hpcugent/easybuild-easyblocks/blob/master/easybuild/easyblocks/w/wrf.py)).

We will present and discuss the [`EB_WRF` easyblock](#wrf-easyblock) that implements the (very non-standard) WRF build procedure, and how to make sure
that EasyBuild can find it and use it.

Once the easyblock is implemented and supplied to EasyBuild we present a [WRF easyconfig file](#wrf-easyconfig) that specifies the install specifications to EasyBuild,
and show how easy it is to build a particular WRF version once the necessary support is in place.


<a name="wiki-wrf-easyblock">
## Adding support for building WRF: writing an easyblock

The Python code below shows the easyblock that implements the support for building WRF,
which is a trimmed down version of the [WRF easyblock provided with EasyBuild](https://github.com/hpcugent/easybuild-easyblocks/blob/master/easybuild/easyblocks/w/wrf.py).

We discuss this Python module block by block.

#### Import statements

```python
import fileinput, os, re, sys

import easybuild.tools.environment as env
from easybuild.easyblocks.netcdf import set_netcdf_env_vars
from easybuild.framework.easyblock import EasyBlock
from easybuild.framework.easyconfig import MANDATORY
from easybuild.tools.filetools import patch_perl_script_autoflush, run_cmd, run_cmd_qa
from easybuild.tools.modules import get_software_root
```

The import statements make sure that all the standard Python modules and functionality provided by EasyBuild that we require is available.
How the various classes and functions provided by EasyBuild are use is shown in the next sections.

#### Class definition, class constructor and defining extra easyconfig options

```python
class EB_WRF(EasyBlock):

  def __init__(self, *args, **kwargs):
    super(EB_WRF, self).__init__(*args, **kwargs)
    self.build_in_installdir = True

  @staticmethod
  def extra_options():
    extra_vars = [('buildtype', [None, "Type of build (e.g., dmpar, dm+sm).", MANDATORY])]
    return EasyBlock.extra_options(extra_vars)
```

The `EB_WRF` class is derived from the 'abstract' easyblock class `EasyBlock` in this case, since there are no (generic) easyblocks that provide
functionality that can be reused for supporting the WRF build and procedure (did we already mention that it's quite non-standard?).
The class naming encoding scheme is documented [here](https://github.com/hpcugent/easybuild/wiki/Encode-class-names).

The generic easyblocks provided by EasyBuild that can serve as base class for an easyblock are hosted in the
[easybuild.easyblocks.generic package](https://github.com/hpcugent/easybuild-easyblocks/blob/master/easybuild/easyblocks/generic).

The class constructor `__init__` simply calls the parent constructor, and initializes a class variable `build_in_installdir` to indicate to EasyBuild that
WRF lacks an actual installation step. This will make EasyBuild unpack the sources and run the build procedure in the install directory, thus making
an actual installation step not necessary.

By defining the static method `extra_options` and passing a list of extra easyconfig options to the `extra_options` method of `EasyBlock`, we can provide 
additional WRF-specific easyconfig parameters that can steer the build procedure.

#### Configuration, part 1: preparation configuration

```python
  def configure_step(self):
    # prepare to configure
    set_netcdf_env_vars(self.log)

    jasper = get_software_root('JasPer')
    jasperlibdir = os.path.join(jasper, "lib")
    if jasper:
      env.setvar('JASPERINC', os.path.join(jasper, "include"))
      env.setvar('JASPERLIB', jasperlibdir)

    env.setvar('WRFIO_NCD_LARGE_FILE_SUPPORT', '1')

    patch_perl_script_autoflush(os.path.join("arch", "Config_new.pl"))

    known_build_types = ['serial', 'smpar', 'dmpar', 'dm+sm']
    self.parallel_build_types = ["dmpar", "smpar", "dm+sm"]
    bt = self.cfg['buildtype']

    if not bt in known_build_types:
      self.log.error("Unknown build type: '%s' (supported: %s)" % (bt, known_build_types))
```

The first part of the configuration as implemented in the `configure_step` method is basically preparation for the actual configuration shown below.

It consists of setting various environment variables for the WRF dependencies netCDF (via the `set_netcdf_env_vars` function provided by the netCDF easyblcok) and JasPer,
indicating that large file support should be enabled and checking whether the specified build type is sensible.

Using the `patch_perl_script_autoflush` provided by EasyBuild we patch the interactive `Config_new.pl` Perl script to allow EasyBuild to perform the configuration
fully autonomously (see below).

#### Configuration, part 2: running configure script and patching resulting config file

```python
    # run configure script
    bt_option = "Linux x86_64 i486 i586 i686, ifort compiler with icc"
    bt_question = "\s*(?P<nr>[0-9]+).\s*%s\s*\(%s\)" % (bt_option, bt)

    cmd = "./configure"
    qa = {"(1=basic, 2=preset moves, 3=vortex following) [default 1]:": "1",
          "(0=no nesting, 1=basic, 2=preset moves, 3=vortex following) [default 0]:": "0"}
    std_qa = {r"%s.*\n(.*\n)*Enter selection\s*\[[0-9]+-[0-9]+\]\s*:" % bt_question: "%(nr)s"}
    
    run_cmd_qa(cmd, qa, no_qa=[], std_qa=std_qa, log_all=True, simple=True)

    # patch configure.wrf
    cfgfile = 'configure.wrf'

    comps = {
             'SCC': os.getenv('CC'), 'SFC': os.getenv('F90'),
             'CCOMP': os.getenv('CC'), 'DM_FC': os.getenv('MPIF90'),
             'DM_CC': "%s -DMPI2_SUPPORT" % os.getenv('MPICC'),
            }
    for line in fileinput.input(cfgfile, inplace=1, backup='.orig.comps'):
      for (k, v) in comps.items():
        line = re.sub(r"^(%s\s*=\s*).*$" % k, r"\1 %s" % v, line)
      sys.stdout.write(line)
```

The second part of the configuration consits of running the `configure` script (which is not a script generated by `autoconf`, as one would expect).
The script is run using the `run_cmd_qa` function provided by EasyBuild which provides support for running interactive scripts fully autonomously.
It sufficies to provide two dictionaries, one with simple string patterns and one with regular expression patterns as keys, that map questions to answers.
In this particular case, we even extract the answer (the number of an item in a list) from the question, see the named group `nr` defined in `bt_question`
and the value of the only entry in `std_qa` that is passed to `run_cmd_qa`.

Afterwards, the `configure.wrf` configuration file produced by `configure` is patched to correctly set the various compiler entries `SCC`, `SFC`, `CCOMP`, etc.,
which completes the configuration of the WRF build.

#### Building WRF

```python
  def build_step(self):
    """Build WRF using the compile script."""
    par = self.cfg['parallel']
    cmd = "./compile -j %d wrf" % par
    run_cmd(cmd, log_all=True, simple=True, log_output=True)

    # build two test cases to produce ideal.exe and real.exe
    for test in ["em_real", "em_b_wave"]:
      cmd = "./compile -j %d %s" % (par, test)
      run_cmd(cmd, log_all=True, simple=True, log_output=True)

  def install_step(self):
    """No separate installation step for WRF."""
    pass
```

Building WRF consists of simply calling the `compile` script with `wrf` as an argument. We make sure we perform the build in parallel (if requested)
by using the `-j` option. We also build two test cases (`em_real` and `em_b_wave`), to make sure the `ideal.exe` and `real.exe` binaries are also being built.

The install step is just empty, since the build as performed in the installation directory (as specified with the `build_in_installdir` class variable set in the constructor).

#### Sanity check (simplified)

```python
  def sanity_check_step(self):
    """Custom sanity check for WRF."""

    wrf_subdir = "WRFV%s" % self.version.split('.')[0]
    custom_paths = {
                    'files': [os.path.join(wrf_subdir, "main", x) for x in ["ideal.exe", "real.exe", "wrf.exe"]],
                    'dirs': []
                   }

    super(EB_WRF, self).sanity_check_step(custom_paths=custom_paths)
```

The `sanity_check_step` method checks whether the expected files and directories are indeed present after the build and install procedure was performed.
Here, we assume that WRF was built correctly if the `ideal.exe`, `real.exe` and `wrf.exe` binaries are indeed present.

#### What's missing?

In this simplified easyblock, we omitted a couple of things for brevity, including:

* support for other compilers: in the easyblock shown, we assume that the Intel compilers are always used, while the actual `EB_WRF` easyblock provided by EasyBuild
has support for multiple compilers
* testing: the `test_step` function that run the various testcases shipped with WRF is omitted since it's quite involved
* module customization: since the binaries and libraries are in a non-standard place, the module generated by EasyBuild should update the `PATH` and `LD_LIBRARY_PATH`
accordingly; likewise, some environment variables need to be set for the netCDF dependency which is also omitted here

The fully featured easyblock implementing support for building, testing and installing WRF is available
[here](https://github.com/hpcugent/easybuild-easyblocks/blob/master/easybuild/easyblocks/w/wrf.py).

#### Providing easyblock to EasyBuild

To provide the easyblock to EasyBuild so it can be used, you need to add it to the `easybuild.easyblocks` package,
or [set up your own](https://github.com/hpcugent/easybuild/wiki/Setting-up-your-own-easyblocks-repository).

Simply place the Python module that is the easyblock with the appropriate name (`wrf.py` in this case), and EasyBuild should pick it up as needed.

Checking whether EasyBuild finds your easyblock can be done with `eb --list-easyblocks` or `eb --easyblock EB_WRF --avail-easyconfig-params=detailed`.

<a name="wiki-wrf-easyconfig">
## Build and install a particular WRF version: putting together an easyconfig file and using `eb`

Finally, to build and install WRF using EasyBuild, you need to compose a simple easyconfig file that specifies all the details for the build (version,
[compiler toolchain](https://github.com/hpcugent/easybuild/wiki/Compiler-toolchains) to use, etc.).

An example easyconfig file is shown below. Note that we request a distributed parallel build of WRF by setting the build type to `dmpar`.
We also list the various dependencies (assuming EasyBuild already has support for these), and specify various patches that required to correctly build WRF
with the Intel compilers, which are part of the `ictce` compiler toolchain that is selected.

The name of the easyconfig file should follow the naming scheme expected by the EasyBuild dependency resolver (a.k.a. robot), i.e.
`<name>-<version>-<toolchain>-<versionsuffix>`, i.e. `WRF-3.4-ictce-3.2.2.u3-dmpar.eb` for this particular example, so it can also be
used when resolving dependencies for other builds (e.g. WPS, which depends on WRF).

```python
name = 'WRF'
version = '3.4'

homepage = 'http://www.wrf-model.org'
description = 'Weather Research and Forecasting'

tcver = '3.2.2.u3'
toolchain = {'name': 'ictce','version': tcver}
toolchainopts = {'opt': False, 'optarch': False}

sources = ['%sV%s.TAR.gz' % (name, version)]

patches = [
    'WRF_parallel_build_fix.patch',
    'WRF-3.4_known_problems.patch',
    'WRF_tests_limit-runtimes.patch',
    'WRF_netCDF-Fortran_separate_path.patch']

dependencies = [('JasPer', '1.900.1'),
                ('netCDF', '4.2'),
                ('netCDF-Fortran', '4.2')]

buildtype = 'dmpar'
versionsuffix = '-%s' % buildtype
```

With that easyconfig file, building WRF is a matter of running a single command with EasyBuild:

```shell
eb WRF-3.4-ictce-3.2.2.u3-dmpar.eb --robot
```

The `--robot` command line option enables the dependency resolves and will make EasyBuild install any missing dependencies before building and installing WRF itself
(assuming it knows how, of course).
