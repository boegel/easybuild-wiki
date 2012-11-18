Configuring [[EasyBuild]] is done by providing a configuration file.

EasyBuild will use the file that is provided by the path/filename in the following order of preference:

* path/filename specified on the EasyBuild command line (using --config or -C),
* path/filename obtained from the environment variable `EASYBUILDCONFIG` (if it is defined)
* the (default) configuration file at `<path where EasyBuild was installed>/easybuild/easybuild_config.py`

EasyBuild expects the configuration file to contain valid Python code, because it executes its contents (using `exec`).
The rationale is that this approach provides a lot of flexibility for configuring EasyBuild.


## Inner workings of the configuration

The configuration file must define the following five variables: `build_path`, `install_path`, `source_path`, `repository`, and `log_format`.
If one of them is not defined, EasyBuild will complain and exit.


### Build path

The `build_path` variable specifies the directory in which EasyBuild builds its software packages.

Each software package is (by default) built in a subdirectory of the `build_path` under `<name>/<version>/<toolkit><versionsuffix>`.

Note that the build directories are emptied by EasyBuild when the installation is completed (by default).


### Install path

The `install_path` variable specifies the directory in which EasyBuild installs software packages and the corresponding module files.

The packages themselves are installed under `install_path/software` in their own subdirectory aptly named `<name>/<version>-<toolkit><versionsuffix>`
(by default), where name is the package name. The corresponding module files are installed under `install_path/modules`.

**Setting MODULEPATH**

After the configuration, you need to make sure that `MODULEPATH` environment variable is extended with the `modules/all` subdirectory of the `install_path`, i.e.:

```bash
export MODULEPATH=<install_path>/modules/all:$MODULEPATH
```

It is probably a good idea to add this to your (favourite) shell .rc file, e.g.,  `.bashrc`, and/or the `.profile` login scripts,
so you do not need to adjust the `MODULEPATH` variable every time you start a new session.


### Source path

The `source_path` variable specifies the directory in which EasyBuild looks for software source and install files.

Similarly to the configuration file lookup, EasyBuild looks for the installation files as given by the `sources` variable
in the .eb easyconfig file, in the following order of preference:

* `<source_path>/<name>`: a subdirectory determined by the name of the software package
* `<source_path>/<letter>/<name>`: in the style of the `easyblocks`/`easyconfigs` directories:
  in a subdirectory determined by the first letter (in lower case) of the software package and by its full `name`
* `<source_path>`: directly in the source path

Note that these locations are also used when EasyBuild looks for patch files in addition to the various `easybuild/easyconfigs`
directories that are listed in the PYTHONPATH.


### Reconfiguration using environment variables

You can (temporarily) override both the `build_path`, `source_path` and `install_path` settings by defining the `EASYBUILDBUILDPATH`,
`EASYBUILDSOURCEPATH` and `EASYBUILDINSTALLPATH` environment variables.

Overriding the configuration file is commonly done when testing new easyblocks.


### Easyconfigs repository

EasyBuild has support for keeping track of (tested) .eb easyconfigs. These files are build specification files for software package installation.
After successfully installing a software package using EasyBuild, the corresponding .eb file is uploaded to a repository defined by the `repository` configuration variable.

Currently, EasyBuild supports the following repository types:

* `FileRepository('path', 'subdir/path'))`: a plain flat file repository; `path` is the path where files will be stored.
* `GitRepository('path', 'path/in/repository'`: a _non-empty_ **bare** git repository (created with `git init --bare` or `git clone --bare`);
   `path` is the path to the git repository (can also be a URL), `path/in/repository` is a path inside the repository where to save the files
* `SvnRepository('path')`: an SVN repository; `path` contains the subversion repository location, again, this can be a directory or a URL

You need to set the `repository` variable inside the config like so:
```python
repository = FileRepository(path)
```

Or, optionally a subdir argument can be specified:

```python
repository = FileRepository(repositoryPath, subdir)
```

You don't have to worry about importing these classes, EasyBuild will make them available to the config file.

Using `git` requires the `GitPython` Python modules, using `svn` requires the `pysvn` Python module (see [[Dependencies]]).

If access to the easyconfigs repository fails for some reason (e.g., no network or a required Python module), EasyBuild will
issue a warning. The software package will still be installed, but the (successful) easyconfig will not be automatically added to the repository.


### Log format

The `log_format` variable contains a tuple specifying a log directory name and a template string. In both of these values, using the following fields is supported:

* `name`: the name of the software package to install
* `version`: the version of the software package to install
* `date`: the date on which the installation was performed (in `YYYYMMDD` format, e.g. `20120324`)
* `time`: the time at which the installation was started (in `HHMMSS` format, e.g. `214359`)

Example :

```python
log_format = ("easylog", "easybuild-%(name)s.log")
```

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

log_format = ("easybuildlogs", "%(name)s-%(version)s.log")`
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

