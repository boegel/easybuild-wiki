Since EasyBuild v2.1, **experimental** support is available for building and installing software on Cray systems
using the ```PrgEnv``` modules provided by Cray. This page provides an overview of the current status.

For more information about this, contact Kenneth Hoste (HPC-UGent, Belgium; kenneth.hoste@ugent.be) and Petar Forai
(IMBA/IMP, Austria; petar.forai@imp.ac.at).

Thanks to Olli-Pekka Letho (CSC.fi), Tim Robinson and Guilherme Peretti-Pezzi (CSCS.ch), and Eric Bavier (Cray) for
providing us access to Cray systems and for their support.

## Test systems

* Piz Daint @ CSCS.ch (http://www.cscs.ch/computers/piz_daint/)
* Sisu @ CSC.fi (https://research.csc.fi/sisu-supercomputer)

## EasyBuild toolchains

* CrayGNU: ```PrgEnv-gnu``` (+ ```cray-libsci```) + ```fftw```
* CrayIntel: ```PrgEnv-intel``` (+ ```cray-libsci```) + ```fftw```
* CrayCCE: ```PrgEnv-cray``` (+ ```cray-libsci```) + ```fftw```

versions:

* ```Cray*/5.1.29```: ```PrgEnv-*/5.1.29``` + ```fftw/3.3.4.0``` (@ Piz Daint)
* ```Cray*/5.2.25```: ```PrgEnv-*/5.2.25``` + ```fftw/3.3.4.2``` (@ Sisu)

## What works already?

(see below for more information)

* building and instaling the **HPL (LINPACK) benchmark** version 2.1
  * on Piz Daint: using ```CrayGNU```, ```CrayIntel``` and ```CrayCCE``` toolchains version 5.1.29
  * on Sisu: using ```CrayGNU```, ```CrayIntel``` and ```CrayCCE``` toolchains version 5.2.25
* building and installing major scientific software applications
  * **CP2K** 2.6.0
    * on Piz Daint using ```CrayGNU/5.1.29```
    * on Sisu using ```CrayGNU/5.2.25```
  * **GROMACS** 4.6.7
    * on Piz Daint using ```CrayGNU/5.1.29```
    * on Sisu using ```CrayGNU/5.2.25```
  * **Python** 2.7.9 + **numpy** 1.9.2 + **scipy** 0.15.1
    * on Piz Daint using ```CrayGNU/5.1.29```
    * on Sisu using ```CrayGNU/5.2.25```
  * **WRF** 3.6.1
    * on Piz Daint using ```CrayGNU/5.1.29```
    * *(pending on Sisu)*

## Required EasyBuild configuration

* enabling experimental mode using ```--experimental``` is *required*
  * support for using the Cray toolchains is still experimental, while we collect feedback and suggestions from experienced users of Cray systems
* the ```craype-*``` module to load *must* be specified using ```--optarch```
  * e.g., ```--optarch=sandybridge``` results in ```craype-sandybridge``` being loaded in the build environment used by EasyBuild
* metadata for external (Cray-provided) modules can be specified using ```--external-modules-metadata```

## Notes

### Modules

* all system-provided loaded modules were purged prior to using EasyBuild:
```
module purge
```
* modules tool used for testing EasyBuild:
  * Sisu: self-installed Lmod 5.8
  * Piz Daint: system-provided environment modules 3.2.10:
```
source /opt/modules/3.2.10.2/init/bash
export PATH=/opt/modules/3.2.10.2/bin/:$PATH
```

## Major supported/tested applications

(in alphabetical order)

### CP2K

* http://www.cp2k.org
* *version(s)*: 2.6.0
* *works on*: Piz Daint (```CrayGNU/5.1.29```), Sisu (```CrayGNU/5.2.25```)
* *runtime tested*: not yet

```
eb CP2K-2.6.0-CrayGNU-5.1.29.eb --dr --optarch=sandybridge --experimental
```

### GROMACS

* http://www.gromacs.org
* *version(s)*: 4.6.7
* *works on*: Piz Daint (```CrayGNU/5.1.29```), Sisu (```CrayGNU/5.2.25```)
* *runtime tested*: not yet

```
eb GROMACS-4.6.7-CrayGNU-5.1.29-mpi.eb --dr --optarch=sandybridge --experimental
```
### HPL

* http://www.netlib.org/benchmark/hpl
* *version(s)*: 2.1
* *works on*: Piz Daint (```CrayGNU/5.1.29```), Sisu (```CrayGNU/5.2.25```)
* *runtime tested*: yes (using default input file, CrayGNU build on Piz Daint)
```
eb HPL-2.1-CrayCCE-5.1.29.eb --dr --optarch=sandybridge --experimental
eb HPL-2.1-CrayGNU-5.1.29.eb --dr --optarch=sandybridge --experimental
eb HPL-2.1-CrayIntel-5.1.29.eb --dr --optarch=sandybridge --experimental
```

### Python + numpy/scipy

* http://python.org, http://www.numpy.org, http://www.scipy.org
* *version(s)*: Python 2.7.9, numpy 1.9.2, scipy 0.15.1
* *works on*: Piz Daint (```CrayGNU/5.1.29```), Sisu (```CrayGNU/5.2.25```)
* *runtime tested*: not yet (except for numpy/scipy tests run during installation)
```
# note: Python and numpy will be installed as dependencies
eb scipy-0.15.1-CrayGNU-5.1.29-Python-2.7.9.eb --dr --optarch=sandybridge --experimental
```

### WRF

* http://www.wrf-model.org
* *version(s)*: 3.6.1
* *works on*: Piz Daint (```CrayGNU/5.1.29```)
* *runtime tested*: yes (using http://www2.mmm.ucar.edu/wrf/WG2/benchv3/)

```
eb WRF-3.6.1-CrayGNU-5.1.29-dmpar.eb --dr --optarch=sandybridge --experimental
```