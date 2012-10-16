Please note that even if you are impatient, you should install all required [[Dependencies]]. On top of that, this demo requires both a C and a C++ compiler to be available on your system, for example `gcc` and `g++`.

To see [[EasyBuild]] in action, build [HPL](http://www.netlib.org/benchmark/hpl/) with the robot feature of EasyBuild by running the following (bash/sh syntax):

```bash
export EBHOME="<path to where you unpacked EasyBuild>/easybuild"
${EBHOME}/eb --robot ${EBHOME}/easybuild/easyconfigs ${EBHOME}/easybuild/easyconfigs/h/HPL/HPL-2.0-goalf-1.1.0.eb
```

This will build and install HPL, after building and installing a GCC-based compiler toolkit and all of its dependencies using the default EasyBuild configuration, which will install to `$HOME/.local/easybuild/software`.

The whole process should take about an hour on a recent system.

Module files will be provided in `$HOME/.local/easybuild/modules/all`, so to load the provided modules, update your `MODULEPATH` environment variable.

If you want to change the path in which easybuild installs the software, read
on!
