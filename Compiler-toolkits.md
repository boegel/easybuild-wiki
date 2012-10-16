Easybuild comes with two compiler toolkits: ictce and goalf; for intel and gcc compilers respectively.
Below is the config file for ictce toolkit:
```python
easyblock="Toolkit"

name='ictce'
version='4.0.6'

homepage='http://software.intel.com/en-us/intel-cluster-toolkit-compiler/'
description="""Intel Cluster Toolkit Compiler Edition provides Intel C,C++ and fortran compilers, Intel MPI and Intel MKL"""

toolkit={'name':'dummy','version':'dummy'}

dependencies=[('icc','2011.6.233'),
              ('ifort','2011.6.233'),
              ('impi','4.0.2.003'),
              ('imkl','10.3.6.233')
             ]

## compiler class
moduleclass='compiler'
```

As you see these toolkits are very simple. They are just a virtual package
with dependencies so that when you use these toolkits, they are automatically
loaded.

## Usage ##

Using a toolkit is very straightforward. You set
`toolkit={'name':'ictce', 'version':'4.0.6'}` in an easyconfig file. Packages
built with different toolkits will have different names so you need not worry
about possible conflicts.

## Create your own ##

Creating your own is very straightforward. Copy paste the above example, and
adjust name and dependencies as needed. Make sure to also look at the icc config
file, so you get an idea how to create a custom compiler.

Remember: you will also need to set up all the dependencies, because they are
the actual workhorses in the toolkit.
