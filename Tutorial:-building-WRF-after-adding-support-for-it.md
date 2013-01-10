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

#### Imports

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

The generic easyblocks provided by EasyBuild that can serve as base class for an easyblock are hosted in the
[easybuild.easyblocks.generic package](https://github.com/hpcugent/easybuild-easyblocks/blob/master/easybuild/easyblocks/generic).

The class constructor `__init__` simply calls the parent constructor, and initializes a class variable `build_in_installdir` to indicate to EasyBuild that
WRF lacks an actual installation step. This will make EasyBuild unpack the sources and run the build procedure in the install directory, thus making
an actual installation step not necessary.

By defining the static method `extra_options` and passing a list of extra easyconfig options to the `extra_options` method of `EasyBlock`, we can provide 
additional WRF-specific easyconfig parameters that can steer the build procedure.

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

```python
  def sanity_check_step(self):
    """Custom sanity check for WRF."""

    wrf_subdir = "WRFV%s" % self.version.split('.')[0]
    custom_paths = {
                    'files': [os.path.join(wrf_subdir, "main", "wrf.exe")],
                    'dirs': []
                   }

    super(EB_WRF, self).sanity_check_step(custom_paths=custom_paths)
```



<a name="wiki-wrf-easyconfig">
## Making EasyBuild build and install a particular WRF version: putting together an easyconfig file

```
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

```
