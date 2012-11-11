Frequently asked questions:

***

## Installation problems

### error: Couldn't find a setup script in easybuild

If you're seeing a problem ```error: Couldn't find a setup script in easybuild``` when installing EasyBuild with ```easy_install```, you are most likely running the command in a directory which has a subdirectory named `easybuild`. 

In that case, ```easy_install``` is trying to find a `setup.py` script in the `easybuild` directory, while you probably want it to go look for EasyBuild on PyPi.

**Solution:** Run the ```easy_install``` command line in a directory which doesn't have a subdirectory named `easybuild`.


***

## Using EasyBuild

### Can I build different numpy and scipy versions? ##

Yes, use the PythonPackageModule to build different version without rebuilding
Python.

### Can I create module files which just load a set of modules? ##

Yes, if you create an easyconfig file with no sources and only dependencies.
You could ofcourse also just create the module file yourself.

### Can I add a dependency on an os specific package (rpm, deb) ##

Yes, but better if you not, they are not really portable:
A better way around is to actually create a .eb file for the dependency
without a sources field.

You can add a sanitycheckcommand to check if the package is available
(it would be nice if this is distribution independend)

This way
* a build will fail without the os dependency available.
* you can later override default tcsh with a custom build one when in
need
* reduce dependencies on OS repositories and distro specific mechanisms
* actually report problems when there are exact version dependencies
(otherwise a default yum/repo update can break a working HPC
application).

### Easybuild complains about an OS dependency, yet I am certain it is installed

Currently easybuild's support for OS dependencies is lacking. It will try to
find it using rpm and dpgk, but this is not perfect.
If you know what you are doing, you can remove the OS dependency from the
easyconfig file, and then build it.

We are trying to remove as much os dependencies as possible, however we can not (yet) build an entire linux system from scratch.

***

## Developing and contributing

### How do I contribute my own easyconfig files? ##

If you want your own easyconfig files to be included with easybuild, you must
fork the [easyconfigs repo](http://github.com/hpcugent/easybuild-easyconfigs/) and send a pull
request.  Note: pull requests should be send to the `develop` branch instead of
the `master` branch.

### I don't like the a-z/ directory structure, can I change it? ##

Easybuild tries to be smart about this and will look inside the $PYTHONPATH to
find the required files. The a-z/ directory structure is entirely optional, but
suggested as it keeps things organized.

Every block inside the `easybuild.easyblocks` namespace can be used. 
In our repo we extend the search path in the `easybuild.easyblocks` namespace (in the `__init__.py`) to also search in the a-z subdirectories.

If you setup your own repo without these subdirectories, keep in mind to put the original repo first in the Python search path, this way we can still extend the search path first.
