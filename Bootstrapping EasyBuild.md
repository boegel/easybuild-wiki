Installing Python packages can be a real pain, and since EasyBuild is a set of Python packages (framework, easyblocks, and easyconfigs on the side) glued together, installing EasyBuild may, ironically, also cause some head aches.

To resolve this, we've created a bootstrap script that installs the latest EasyBuild version for you together with an environment module for it (and yes, we use EasyBuild for doing so).

All you need is Python 2.4 (or a more recent 2.x) and environment modules installed on your system.

### In case of problems...

**Please open an issue:** https://github.com/hpcugent/easybuild/issues/new

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

Download the bootstrap script, and run it, specifying an install path for EasyBuild:

```shell
curl -O https://raw.githubusercontent.com/hpcugent/easybuild-framework/develop/easybuild/scripts/bootstrap_eb.py
python bootstrap_eb.py $HOME/.local/easybuild
```

**Note**: The path you specify to the bootstrap script is where EasyBuild should be installed. If you also want software that is built using EasyBuild to be installed there, you'll need to set `EASYBUILDINSTALLPATH`, and/or look into the [details on configuring EasyBuild](https://github.com/hpcugent/easybuild/wiki/Configuration).

### Sanity check

**Please provide the results of the commands below as well, to verify that everything worked.**

Set your `MODULEPATH` correctly if needed, and load the EasyBuild module with the specific version (see output of bootstrap script for more details):

```shell
export MODULEPATH=$HOME/.local/easybuild/modules/all:$MODULEPATH
module load EasyBuild/<version>
```

Determine the version of the installed EasyBuild, which should match the name of the module:

```shell
eb --version
```

### Running unit tests

After completion of the bootstrap procedure and loading the `EasyBuild` module, try running the EasyBuild unit tests. If this doesn't complete successfully, **[please open an issue](https://github.com/hpcugent/easybuild-framework/issues/new)** to report it.

```
python -m test.framework.suite
```