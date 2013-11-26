# EasyBuild configuration

(this page discusses the new style of configuring EasyBuild, which is supported since EasyBuild v1.3.0; for the legacy way of configuring EasyBuild, see [here](https://github.com/hpcugent/easybuild/wiki/Configuration-legacy))

Configuring EasyBuild can be done using:
* `eb` with **command line arguments**
* setting **environment variables**
* providing a **configuration file**

Of course, combining any of these types of configuration works too (and is even fairly common).

The order of preference for the different configuration types is a listed above, i.e., environment variables override the corresponding entries in the configuration file,
while command line arguments in turn override the corresponding environment variables _and_ matching entries in the configuration file.


## Consistentency across supported configuration types

Note that the various available configuration options are handled **consistently** across the supported configuration types, i.e. for defining the configuration setting `foo-option` to `bar`, the following alternatives are available:
 * configuration file entry (key-value assignment):
```python
foo-option = bar
```
 * environment variable (upper case, `EASYBUILD_` prefix, `-`s become `_`s):
```bash
$ export EASYBUILD_FOO_OPTION=bar
$ echo $EASYBUILD_FOO_OPTION
bar
```
 * command line argument (long options preceded by `--` and using `=`):
```bash
$ eb --foo-option=bar
```

For more details w.r.t. each of the supported configuration types, see below.


## Available configuration settings

To obtain a full and up-to-date list of available configuration settings, see `eb --help`.
We refrain from listing all available configuration settings here, to avoid outdated documentation.

A couple of selected configuration settings are discussed below, in particular the mandatory settings.


### Mandatory configuration settings

A handful of configuration settings are **mandatory**, and should be provided using one of the supported configuration types.

The following configuration settings are currently mandatory:
 * source path
 * build path
 * install path
 * easyconfigs repository
 * format for name of logfile

If any of these configuration settings is not provided in one way or another, EasyBuild will complain and exit. In practice, all of these have reasonable defaults.


##### Source path

**default**: `$HOME/.local/easybuild/sources/`

The `sourcepath` configuration setting specifies the directory in which EasyBuild looks for software source and install files.

Looking for the files specified via the `sources` parameter in the .eb easyconfig file is done in the following order of preference:

* `<sourcepath>/<name>`: a subdirectory determined by the name of the software package
* `<sourcepath>/<letter>/<name>`: in the style of the `easyblocks`/`easyconfigs` directories:
  in a subdirectory determined by the first letter (in lower case) of the software package and by its full `name`
* `<sourcepath>`: directly in the source path

Note that these locations are also used when EasyBuild looks for patch files in addition to the various `easybuild/easyconfigs`
directories that are listed in the PYTHONPATH.


##### Build path

**default**: `$HOME/.local/easybuild/build/`

The `buildpath` configuration setting specifies the (temporary) directory in which EasyBuild builds its software packages.

Each software package is (by default) built in a subdirectory of the specified `buildpath` under `<name>/<version>/<toolkit><versionsuffix>`.

Note that the build directories are emptied and removed by EasyBuild when the installation is completed (by default).

Tip: using `/dev/shm` as build path can significantly speed up builds, if it is available and provides a sufficient amount of space.


##### Install path

**default**: `$HOME/.local/easybuild/`

The `installpath` configuration setting specifies the directory in which EasyBuild installs software packages and the corresponding module files.

The packages themselves are installed under `install_path/software` in their own subdirectory following the active module naming scheme (e.g.,
`<name>/<version>-<toolkit><versionsuffix>`, by default). The corresponding module files are installed under `install_path/modules`.

**Setting MODULEPATH**

After the configuration, you need to make sure that `MODULEPATH` environment variable is extended with the `modules/all` subdirectory of the `installpath`, i.e.:

```bash
export MODULEPATH=<installpath>/modules/all:$MODULEPATH
```

It is probably a good idea to add this to your (favourite) shell .rc file, e.g., `~/.bashrc`, and/or the `~/.profile` login scripts,
so you do not need to adjust the `MODULEPATH` variable every time you start a new session.


##### Easyconfigs repository

**default**: `FileRepository` at `$HOME/.local/easybuild/ebfiles_repo`

EasyBuild has support for keeping track of (tested) .eb easyconfig files.
After successfully installing a software package using EasyBuild, the corresponding .eb file is uploaded to a repository defined by the `repository` and `repositorypath` configuration settings.

Currently, EasyBuild supports the following repository types (see also `eb --avail-repositories`):

* `FileRepository('path', 'sub/dir'))`: a plain flat file repository; `path` is the path where files will be stored.
* `GitRepository('path', 'path/in/repository'`: a _non-empty_ **bare** git repository (created with `git init --bare` or `git clone --bare`);
   `path` is the path to the git repository (can also be a URL), `path/in/repository` is a path inside the repository where to save the files
* `SvnRepository('path')`: an SVN repository; `path` contains the subversion repository location, again, this can be a directory or a URL

You need to set the `repository` setting inside a configuration file like this:
```python
repository = FileRepository
repositorypath = path
```

Or, optionally an extra argument representing a subdirectory can be specified:

```bash
$ export EASYBUILD_REPOSITORY=FileRepository
$ export EASYBUILD_REPOSITORYPATH=path,sub/dir
```

You don not have to worry about importing these classes, EasyBuild will make them available to the configuration file.

Using `git` requires the `GitPython` Python modules, using `svn` requires the `pysvn` Python module (see [[Dependencies]]).

If access to the easyconfigs repository fails for some reason (e.g., no network or a required Python module), EasyBuild will
issue a warning. The software package will still be installed, but the (successful) easyconfig will not be automatically added to the repository.


##### Logfile format

**default**: `easybuild, easybuild-%(name)s-%(version)s-%(date)s.%(time)s.log`

The `logfile-format` configuration setting contains a tuple specifying a log directory name and a template log file name.
In both of these values, using the following fields is supported:

* `name`: the name of the software package to install
* `version`: the version of the software package to install
* `date`: the date on which the installation was performed (in `YYYYMMDD` format, e.g. `20120324`)
* `time`: the time at which the installation was started (in `HHMMSS` format, e.g. `214359`)

For example, the logfile format can be specified as follows in the EasyBuild configuration file:

```python
logfile-format = "easylog", "easybuild-%(name)s.log"
```



### Optional configuration settings


##### Software and modules install path suffixes

(supported since v1.1.0)

**defaults**: `software` as software install path suffix, `modules` as modules install path suffix

The software and modules install path suffixes can be adjusted using the `subdir-software` and/or `subdir-modules` configuration settings, for example:

```bash
$ export EASYBUILD_SUBDIR_SOFTWARE=installs
$ eb --subdir-modules=module_files ...
```

Note: EasyBuild will still use the additional `all` and `base` suffixes for the module install paths, along with a directory for every module class that is being used.


##### Modules tool

**default**: `EnvironmentModulesC`

Specifying the modules tool that should be used by EasyBuild can be done using the `modules-tool` configuration setting.
A list of supported modules tools can be obtained using `eb --avail-modules-tools`, example include: `EnvironmentModulesC`, `EnvironmentModulesTcl`, `Lmod`, etc.

For example, to indicate that EasyBuild should be using `Lmod` as modules tool:

```bash
export EASYBUILD_MODULES_TOOL=Lmod
```


##### Active module naming scheme

**default**: `EasyBuildModuleNamingScheme`

The module naming scheme that should be used by EasyBuild can be specified using the `module-naming-scheme` configuration setting.

More details are available on the dedicated wiki page [[Using a custom module naming scheme]].


## Supported configuration types


### Configuration file

The EasyBuild configuration file follows the default Python configuration format as parsed by the `configparser` module (see [http://docs.python.org/2/library/configparser.html](http://docs.python.org/2/library/configparser.html)).

The set of configuration files that will be used by EasyBuild is determined in the following order of preference:
* the path(s) specified via **command line argument `--configfiles`**
* the path(s) specified via the **`$EASYBUILD_CONFIGFILES` environment variable**
* the **default path** for the EasyBuild configuration file, i.e. `$HOME/.easybuild/config.cfg`

On top of this, the command line argument `--ignoreconfigfiles` allows to specify configuration files that should be _ignored_ by EasyBuild (regardless of whether they are specified via any of the options above).


### Environment variables


### Command line arguments


### Legacy configuration options (deprecated)

In EasyBuild v1.x, a couple of configuration options other than the ones below are available that follow the **legacy configuration style**, including:
* the `-C` and `--config` command line arguments
* the `$EASYBUILDCONFIG` environment variable
* the default path `$HOME/.easybuild/config.py`
* the legacy fallback path `<installpath>/easybuild/easybuild_config.py`

_**We _strongly_ advise to switch to the new way of configuring EasyBuild as soon as possible, since the legacy style will no longer be supported in EasyBuild v2.x.**_


### Default configuration






<a name="wiki-example_config">
## Example configuration

This is a simple example configuration file, that specifies the user's home directory as the `install_path` and  that uses /tmp/easybuild as the `build_path`.
Additionally, it states that the .eb repository resides in $HOME/ebfiles and that it stores flat files:

```python
import os

home = os.getenv('HOME')

build_path = "/tmp/easybuild"
install_path = home
source_path = os.path.join(home, "sources")

repository_path = os.path.join(home, "ebfiles")
repository = FileRepository(repository_path)

log_format = ("easybuildlogs", "%(name)s-%(version)s.log")

module_classes = ['base', 'bio', 'chem', 'compiler', 'lib', 'phys', 'tools']
```


## Default configuration

The default configuration file that comes with EasyBuild (`easybuild/easybuild_config.py`) can be used as a starting point if you create your own configuration file,
see [here](https://github.com/hpcugent/easybuild-framework/tree/master/easybuild/easybuild_config.py).


### Paths

In the default configuration file, the prefix for the build, install and source directories is obtained from the `EASYBUILDPREFIX` environment variable.
If this variable was not defined, the prefix is set to $HOME/.local/easybuild/

If for some reason `HOME` is not defined, the configuration file falls back to using `/tmp/easybuild` as the prefix for the various paths.

These paths are then defined as follows:

* `build_path`: `<prefix>/easybuild/build`
* `install_path`: `<prefix>/easybuild`
* `source_path`: `<prefix>/easybuild/sources`


### Easyconfigs repository

The `repository_path` is specified as `<prefix>/ebfiles_repo`, and
`repository` is set to `FileRepository`


### Log format

The default log format uses all fields available:

```python
("easybuildlog", "easybuild-%(name)s-%(version)s-%(date)s.%(time)s.log")
```

## Reconfiguration using environment variables

You can (temporarily) override both the `build_path`, `source_path` and `install_path` settings by defining the `EASYBUILDBUILDPATH`,
`EASYBUILDSOURCEPATH` and `EASYBUILDINSTALLPATH` environment variables.

Overriding the configuration file is commonly done when testing new easyblocks.
