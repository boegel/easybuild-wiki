
## easybuild\_config.py

```python
buildPath = '/local/easybuild-dev/build'
sourcePath = '/apps/gent/source'
installPath = '/local/easybuild-dev/install'
repositoryPath = ('svn+ssh://user@code.ugent.be/easybuild/trunk/easybuild/specfiles')
logFormat = ("easybuild", "easybuild-%(name)s-%(version)s-%(date)s.%(time)s.log")
```

## Python-2.7.5-ictce-5.5.0.eb
```python
name = 'Python'
version = '2.7.5'

homepage = 'http://python.org/'
description = "Python is a programming language that lets you work more quickly and integrate your systems more effectively."

toolchain = {'name': 'ictce', 'version': '5.5.0'}
toolchainopts = {'pic': True, 'opt': True, 'optarch': True}

source_urls = ['http://www.python.org/ftp/%(namelower)s/%(version)s/']
sources = ['%(name)s-%(version)s.tgz']

patches = ['python-%(version)s_libffi_int128_icc.patch']

# python needs bzip2 to build the bz2 package
dependencies = [
    ('bzip2', '1.0.6'),
    ('zlib', '1.2.8'),
    ('libreadline', '6.2'),
    ('ncurses', '5.9'),
]

configopts="--with-threads"

moduleclass='lang'
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

