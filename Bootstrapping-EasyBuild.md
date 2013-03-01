Installing Python packages can be a real pain, and since EasyBuild is a set of Python packages (framework, easyblocks, and easyconfigs on the side) glued together, installing EasyBuild may, ironically, also cause some head aches.

To resolve this, we've created a bootstrap script that installs the latest EasyBuild version for you together with an environment module for it (and yes, we use EasyBuild for doing so).

All you need is Python 2.4 (or a more recent 2.x) and environment modules installed on your system.

## Call for testing

**We are currently still testing and evaluating the current bootstrap script, and are primarily looking for experiences with its use by others. Please test the script as outlined below, and provide feedback (mail kenneth.hoste@ugent.be with your findings)!**

### Collect info before running bootstrap script (only when testing)

In order to get a better understanding in which kind of environment you're using the bootstrap script, please copy-paste the commands below and provide the output.
**Don't worry if some of these commands fail or spit out error messages.**

```shell
python -V
module av EasyBuild
which -a eb
eb --version
```

### Bootstrapping EasyBuild

1. Download the bootstrap script from https://raw.github.com/boegel/easybuild-framework/bootstrap/easybuild/scripts/bootstrap_eb.py
2. Run it, specifying an install path for EasyBuild:

```shell
python bootstrap_eb.py /some/path
```

### Sanity check

**Please provide the results of the commands below as well, to verify that everything worked.**

Set your `MODULEPATH` correctly if needed, and load the EasyBuild module with the specific version (see output of bootstrap script for more details):

```shell
export MODULEPATH=/some/path/modules/all:$MODULEPATH
module load EasyBuild/<version>
```

Determine the version of the installed EasyBuild, which should match the name of the module:

```shell
eb --version
```
