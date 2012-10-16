Configuring [[EasyBuild]] is done by providing a configuration file.

EasyBuild will use the file that is provided by the path/filename in the following order of preference:

* path/filename specified on the EasyBuild command line,
* path/filename obtained from the environment variable `EASYBUILDCONFIG` (if it is defined)
* the (default) configuration file at the `<path where EasyBuild was placed>/easybuild/easybuild_config.py`

EasyBuild expects the configuration file to contain valid Python code, because it executes its contents (using `exec`). The rationale is that this approach provides a lot of flexibility for configuring EasyBuild.

## Inner workings of the configuration

The configuration file must define the following six variables: `buildPath`, `installPath`, `sourcePath`, `repository`, `repositoryPath` and `logFormat`.
If one of them is not defined, EasyBuild will complain and bail.

### Build path

The `buildPath` variable specifies the directory in which EasyBuild builds its software packages.

Each software package is built in a subdirectory of the `buildPath` under `<name>/<version>/<toolkit><versionsuffix>`.

Note that the build directories are emptied by EasyBuild when the installation is completed. They are not removed at this point.

### Install path

The `installPath` variable specifies the directory in which EasyBuild installs software packages and the corresponding module files.

The packages themselves are installed under `installPath/software` in their own subdirectory aptly named `<name>/<version>-<toolkit><versionsuffix>`, where name is the package name. The corresponding module files are installed under `installPath/modules`.

**Setting MODULEPATH**

After the configuration, you need to make sure that `MODULEPATH` environment variable is extended with the `modules/all` subdirectory of the `installPath`, i.e.:

```bash
export MODULEPATH=<installPath>/modules/all:$MODULEPATH
```

It is probably a good idea to add this to your (favourite) shell .rc file, e.g.,  `.bashrc`, and/or the `.profile` login scripts, so you do not need to adjust the `MODULEPATH` variable every time you start a new session.

### Source path

The `sourcePath` variable specifies the directory in which EasyBuild looks for software source and install files.

Similarly to the configuration file lookup, EasyBuild looks for the installation files as given by the `sources` variable in the .eb easyconfig file, in the following order of preference:

* `<sourcePath>/<name>`: a subdirectory determined by the name of the software package
* `<sourcePath>/<letter>/<name>`: in the style of the `easyblocks`/`easyconfigs` directories: in a subdirectory determined by the first letter (in lower case) of the software package and by its full `name`
* `<sourcePath>`: directly in the source path

Note that these locations are also used when EasyBuild looks for patch files in addition to the various `easybuild/easyconfigs` directories that are listed in the PYTHONPATH.

### Reconfiguration using environment variables

You can (temporary) override both the `buildPath` and `installPath` settings by defining the `EASYBUILDBUILDPATH` and `EASYBUILDINSTALLPATH` environment variables.

Overriding the configuration file is commonly done when testing new easyblocks.

### Easyconfigs repository

EasyBuild has support for keeping track of (tested) .eb easyconfigs. These files serve configuration files for software package installation. After successfully installing a software package using EasyBuild, the corresponding .eb file is uploaded to a repository defined by the `repository` configuration variable.

Currently, EasyBuild supports the following repository types:

* `FileRepository`: a plain flat file repository. In this case, the `repositoryPath` contains the directory where the files are stored,
* `GitRepository`: a _non-empty_ **bare** git repository (created with `git init --bare` or `git clone --bare`).
   Here, the `repositoryPath` contains the git repository location, which can be a directory or an URL.
* `SvnRepository`: an SVN repository. In this case, the `repositoryPath` contains the subversion repository location, again, this can be a directory or an URL.

you have to set the `repository` variable inside the config like so:
`repository = FileRepository(repositoryPath)`

optionally a subdir argument can be specified:
`repository = FileRepository(repositoryPath, subdir)`

You don't have to worry about these classes, easybuild will make them available
inside the config file.

Using `git` requires the `GitPython` Python modules, using `svn` requires the `pysvn` Python module (see [[Dependencies]]).

If access to the easyconfigs repository fails for some reason (e.g., no network or a required Python module), EasyBuild will issue a warning. The software package will still be installed, but the (successful) easyconfig will not be automatically added to the repository.

### Log format

The `logFormat` variable contains a tuple specifying a log directory name and a template string. In both of these values, using the following fields is supported:

* `name`: the name of the software package to install
* `version`: the version of the software package to install
* `date`: the date on which the installation was performed (in `YYYYMMDD` format, e.g. `20120324`)
* `time`: the time at which the installation was started (in `HHMMSS` format, e.g. `214359`)

Example :

```python
logFormat=("easylog","easybuild-%(name)s.log")
```

## Example configuration

This is a simple example configuration file, that specifies the user's home directory as the `installPath` and  that uses /tmp/easybuild as the `buildPath`. Additionally, it states that the .eb repository tresides in $HOME/ebfiles and that it stores flat files:

```python
import os

home = os.getenv('HOME')

buildPath = "/tmp/easybuild"
installPath = home
sourcePath = os.path.join(home, "sources")

repositoryPath = os.path.join(home, "ebfiles")
repository = FileRepository(repositoryPath)

logFormat = ("easybuildlogs", "%(name)s-%(version)s.log")`
```

## Default configuration

The default configuration file that comes with EasyBuild (`easybuild/easybuild_config.py`) can be used as a starting point if you create your own configuration files.

Below you see the default config file
```python
import os

# buildPath possibly overridden by EASYBUILDBUILDPATH
# installPath possibly overridden by EASYBUILDINSTALLPATH

buildDir = 'build'
installDir = ''
sourceDir = 'sources'

if os.getenv('EASYBUILDPREFIX'):
    prefix = os.getenv('EASYBUILDPREFIX')
else:
    prefix = os.path.join(os.getenv('HOME'), ".local", "easybuild")

if not prefix:
    prefix = "/tmp/easybuild"

buildPath = os.path.join(prefix, buildDir)
installPath = os.path.join(prefix, installDir)
sourcePath = os.path.join(prefix, sourceDir)

repositoryPath = os.path.join(prefix, 'ebfiles_repo')
repository = FileRepository(repositoryPath)

logFormat = ("easybuildlog", "easybuild-%(name)s-%(version)s-%(date)s.%(time)s.log")
```

### Paths

The prefix for the build, install and source directories is obtained from the `EASYBUILDPREFIX` environment variable.

If this variable was not defined, the prefix is set to $HOME/.local/easybuild/

If for some reason `HOME` is not defined, the configuration file falls back to using `/tmp` as the prefix for the various paths.

These paths are then defined as follows:

* `buildPath`: `<prefix>/easybuild_build`
* `installPath`: `<prefix>/easybuild`
* `sourcePath`: `<prefix>/easybuild_sources`

### Easyconfigs repository

The `repositoryPath` is specified as `<prefix>/ebfiles_repo`, and
`repository` is set to `FileRepository`

### Log format

The default log format uses all fields available:

```python
("easybuildlog", "easybuild-%(name)s-%(version)s-%(date)s.%(time)s.log")
```
