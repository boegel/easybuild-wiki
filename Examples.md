
## easybuild\_config.py

```python
buildPath = '/local/easybuild-dev/build'
sourcePath = '/apps/gent/source'
installPath = '/local/easybuild-dev/install'
repositoryPath = ('svn+ssh://user@code.ugent.be/easybuild/trunk/easybuild/specfiles')
logFormat = ("easybuild", "easybuild-%(name)s-%(version)s-%(date)s.%(time)s.log")
```

## Python-2.7-ictce-5.5.0.eb
```python
name='Python'
version='2.7'

homepage='http://python.org/'
description="Python is a programming language that lets you work more quickly and integrate your systems more effectively."

toolkit={'name':'ictce','version':'5.5.0'}
toolkitopts={'pic':True,'opt':True,'optarch':True}

patches=['python_libffi_int128_icc.patch']

osdependencies=['tk-devel']

sources=['%s-%s.tgz'%(name,version)]

configopts="--with-threads"

moduleclass='base'
```

## icc.eb
```python
mod="Intel.Icc"

name='icc'

homepage='http://software.intel.com/en-us/intel-compilers/'
description="C and C++ compiler from Intel"

toolkit={'name':'dummy','version':'dummy'}

## compiler class
moduleclass='compiler'

dontcreateinstalldir=True

## licensepath
license="/apps/gent/licenses/intel"

usetmppath=False

[11.1.069]

version='11.1.069'
patches=['install_specified_icc.patch']
sources=['l_cproc_p_%s_intel64.tgz'%version]

[2011.4.191]

version='2011.4.191'
sources=['l_ccompxe_intel64_%s.tgz'%version]
```

