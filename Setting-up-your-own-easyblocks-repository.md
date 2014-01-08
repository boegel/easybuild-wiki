EasyBuild allows you to host your own set of easyblocks, next to the ones that ship with the
EasyBuild version you have installed.

You can provide these easyblocks to EasyBuild by simply making sure your Python path is set correctly.

EasyBuild will then use these easyblocks when needed, i.e., when requested to build/install software that require them.


## Setting up your own easyblocks repository

The following steps outline how to set up your own easyblocks repository. Of course, you will only need to do this procedure
once on your system.

We present the path in which will host your set of easyblocks by the environment variable $MYEBDIR, e.g.,

```bash
export MYEBDIR=$HOME/easybuild/my_easyblocks
```


### Step 1: Directory structure

In your easyblocks repository, you need to make sure you adhere to the EasyBuild namespace, i.e.,
a top-level directory `easybuild` with a subdirectory `easyblocks`:

```bash
mkdir -p $MYEBDIR/easybuild/easyblocks
```

The Python modules representing your easyblocks should then be hosted directly in the `easyblocks`
directory (not in subdirectories like `a`, `b`, etc., see [below](#searching) why). For example:

```bash
$ ls $MYEBDIR/easybuild/easyblocks
bar.py
foo.py
linuxfromscratch.py
```

### Step 2: Setting up Python packages

The `easybuild/easyblocks` path should be a Python package, so it should contain
an `__init__.py` file. This file can, and should in this case, be empty; see the [note](#searching) on searching
Python modules in Python packages for the reason why they should be empty.

```bash
touch $MYEBDIR/easybuild/easyblocks/__init__.py
```


### Step 3: Setting Python path

After creating your easyblocks repository, all you need to do is make sure that EasyBuild is able
to find your easyblocks, by adding the path to the Python path.

```bash
# extend path for easyblocks for EasyBuild (pro tip: add this to your .bashrc)
export PYTHONPATH=$PYTHONPATH:$MYEBDIR
```

**Note**: Add the path to your easyblocks at the end, not at the beginning, to avoid messing with the `easybuild` namespace (see
[below](#searching) as to why).


<a name="wiki-searching">
### Note: The way in Python modules in Python packages are searched and found

The reason why some notes were made in the steps above is the way in which searching for Python modules
in the Python path, and some custom hacking in `__init__.py` of the `easyblocks` package that comes
with an EasyBuild installation.


#### Flattening the easyblocks namespace in EasyBuild

We organise Python modules representing easyblocks in subdirectories by their first letter, i.e.:

```bash
$ ls easybuild/easyblocks/*/*
easybuild/easyblocks/a/acml.py
easybuild/easyblocks/a/armadillo.py
easybuild/easyblocks/a/atlas.py
easybuild/easyblocks/b/boost.py
...
```

In the `__init__.py` file, we then flatten this again as if all these Python modules were located directly
in the `easybuild/easyblocks` namespace. That allows for cleaner import statements, i.e.:

```python
from easybuild.easyblocks.atlas import EB_ATLAS
```


#### Package initialisation by Python

When Python wanders through the Python path, it will initialise any Python package it comes across by
executing its `__init__.py` file. And here's the catch: Python will only actually execute the **first** `__init__.py`
file it comes across for each package.

That means that if a Python package is spread across multiple directories, as is the case with the
`easybuild/easyblocks` path in the EasyBuild installation path and your own set of easyblocks in a custom directory,
you need to make sure the right `__init__.py` file gets executed. 

That is the reason for adding the path to your set of easyblocks at the end of `PYTHONPATH`, and why your `__init__.py`
files should be empty (they won't get executed anyway, so why bother putting something in there). It also explains why
you should place your easyblocks directly in the `easybuild/easyblocks` subdirectory of your easyblocks repository,
since you can only do the flattening trick once, and EasyBuild relies on the fact that it is able to import the easyblock
for software package Foo with:

```
from easybuild.easyblocks.foo import EB_Foo
```


## Creating easyblocks

When you start working on a new easyblock, you have a couple of options:

* copying and adjusting an existing easyblock, e.g., one shipped with EasyBuild that shows similarities with the support you need
to implement
* starting from scratch, and write an entirely new easyblock (good exercise, but time-consuming to do it every time)
* using the ```mk_tmpl_easyblock_for.py``` script provided with EasyBuild (available as of EasyBuild v1.1 in the
`easybuild/scripts` subdirectory) to generate a template easyblock specific to the software package you want to support

We recommend the last option, since that will allow you to start with a clean slate and limit the amount of things to
get right at the same time.


## Contributing back

Once you finish the work on an easyblock, i.e., it allows you to build a previously unsupported software package and
(preferably) you have done some effort to verify the build, we encourage you to contribute the easyblock and at least one
example easyconfig back to EasyBuild. 

See the wiki page on [[Contributing back]] for more information.

