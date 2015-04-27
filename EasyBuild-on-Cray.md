Since EasyBuild v2.1, **experimental** support is available for building and installing software on Cray systems
using the ```PrgEnv``` modules provided by Cray. This page provides an overview of the current status.

For more information about this, contact Kenneth Hoste (HPC-UGent, Belgium; kenneth.hoste@ugent.be) and Petar Forai
(IMBA/IMP, Austria; petar.forai@imp.ac.at).

Thanks to Olli-Pekka Letho (CSC.fi), Tim Robinson and Guilherme Peretti-Pezzi (CSCS.ch), and Bavier (Cray) for
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

* building and instaling the **HPL (LINPACK) benchmark** version 2.1 ([http://www.netlib.org/benchmark/hpl](http://www.netlib.org/benchmark/hpl))
  * on Piz Daint: using ```CrayGNU```, ```CrayIntel``` and ```CrayCCE``` toolchains version 5.1.29
  * on Sisu: using ```CrayGNU```, ```CrayIntel``` and ```CrayCCE``` toolchains version 5.2.25
* building and installing major scientific software applications
  * **CP2K** 2.6.0 ([http://www.cp2k.org](http://www.cp2k.org))
    * on Piz Daint using ```CrayGNU/5.1.29```
    * on Sisu using ```CrayGNU/5.2.25```
  * **GROMACS** 4.6.7 ([http://www.gromacs.org](http://www.gromacs.org))
    * on Piz Daint using ```CrayGNU/5.1.29```
    * on Sisu using ```CrayGNU/5.2.25```
  * **Python** 2.7.9 ([http://python.org](http://python.org)) + **numpy** 1.9.2 ([http://www.numpy.org](http://www.numpy.org)) + **scipy** 0.15.1 ([http://www.scipy.org](http://www.scipy.org)):
    * on Piz Daint using ```CrayGNU/5.1.29```
    * on Sisu using ```CrayGNU/5.2.25```
  * **WRF** 3.6.1 ([http://www.wrf-model.org](http://www.wrf-model.org)):
    * on Piz Daint using ```CrayGNU/5.1.29```
    * *(pending on Sisu)*

## Required EasyBuild configuration

* enabling experimental mode using ```--experimental``` is *required*
  * support for using the Cray toolchains is still experimental, while we collect feedback and suggestions from experienced users of Cray systems
* the ```craype-*``` module to load *must* be specified using ```--optarch```
  * e.g., ```--optarch=sandybridge``` results in ```craype-sandybridge``` being loaded in the build environment used by EasyBuild
* metadata for external (Cray-provided) modules can be specified using ```--external-modules-metadata```

## Major supported/tested applications

(in alphabetical order)

### CP2K

### GROMACS

### HPL

### Python

with numpy/scipy

### WRF
