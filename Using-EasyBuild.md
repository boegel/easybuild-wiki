 * <a href="#basic-usage">Basic usage</a>
 * <a href="#help">Help</a>
 * <a href="#basic-options">Basic options</a>
 * <a href="#software-build-options">Software build options</a> 
 * <a href="#override-options">Override options</a>
 * <a href="#informative-options">Informative options</a>

***

<a name="#basic-usage"/>
## Basic usage

(**note**: we will use `$EBHOME` to indicate the location where EasyBuild was installed)

Using EasyBuild is really easy: just run the `eb` script and specify which software you want to build.

You can do this either by supplying an [easyconfig file](https://github.com/hpcugent/easybuild/wiki/Easyconfig-files) (`.eb`), or only using command line options and hope that EasyBuild is able to figure out or generate a matching build specification.

Recommended basic usage, by supplying an easyconfig file:

```sh
$EBHOME/eb example.eb
```

With supplying an easyconfig file (_not recommended_, more likely to fail):

```sh
$EBHOME/eb --software-name=gzip
```

EasyBuild will then try to build and install the software following the given specifications.

EasyBuild offers various command line options to access other features, see below.

<a name="help"/>
## `-h`, `--help`: List of available options

To obtain the list of available options, just run the `eb` script with the `-h` or `--help` option, which will provide output as shown below. Running the script without specifying a `.eb` file will also generate this output.

The _usage_ description at the top specifies `build.py` because `eb` is just a wrapper around `easybuild/build.py`, which updates the `PYTHONPATH` to ensure that EasyBuild can work as expected.

```txt
Usage: build.py [options] easyconfig [..]

Builds software package based on easyconfig (or parse a directory). Provide
one or more easyconfigs or directories, use -h or --help more information.

Options:
  -h, --help            show this help message and exit

  Basic options:
    Basic runtime options for EasyBuild.

    -b BLOCKS, --only-blocks=BLOCKS
                        Only build blocks blk[,blk2]
    -d, --debug         log debug messages
    -f, --force         force to rebuild software even if it's already
                        installed (i.e. can be found as module)
    --job               will submit the build as a job
    -k, --skip          skip existing software (useful for installing
                        additional packages)
    -l                  log to stdout
    -r PATH, --robot=PATH
                        path to search for easyconfigs for missing
                        dependencies
    --regtest           enable regression test mode
    --regtest-online    enable online regression test mode
    -s STOP, --stop=STOP
                        stop the installation after certain step (valid: cfg,
                        source, patch, prepare, configure, make, install,
                        test, postproc, cleanup, packages)
    --strict=STRICT     set strictness level (possible levels: ignore, warn,
                        error)

  Software build options:
    Specify software build options (overrides settings in easyconfig).

    --software-name=NAME
                        build software package with given name
    --software-version=VERSION
                        build software with this particular version
    --toolkit=NAME,VERSION
                        build with specified toolkit (name and version)
    --toolkit-name=NAME
                        build with specified toolkit name
    --toolkit-version=VERSION
                        build with specified toolkit version
    --amend=VAR=VALUE[,VALUE2]
                        specify additional build parameters (can be used
                        multiple times); for example: versionprefix=foo or
                        patches=one.patch,two.patch

  Override options:
    Override default EasyBuild behavior.

    -C CONFIG, --config=CONFIG
                        path to EasyBuild config file [default:
                        $EASYBUILDCONFIG or easybuild/easybuild_config.py]
    -e CLASS, --easyblock=CLASS
                        loads the class from module to process the spec file
                        or dump the options for [default: Application class]
    -p, --pretend       does the build/installation in a test directory
                        located in $HOME/easybuildinstall [default:
                        $EASYBUILDINSTALLDIR or installPath in EasyBuild
                        config file]
    -t, --skip-tests    skip testing

  Informative options:
    Obtain information about EasyBuild.

    -a, --avail-easyconfig-params
                        show available easyconfig parameters
    --dump-classes      show list of available classes
    --search=SEARCH     search for module-files in the robot-directory
    -v, --version       show version
    --dep-graph=depgraph.<ext>
                        create dependency graph
```

<a name="basic-options"/>
## Basic options

#### Only build specified blocks: `-b`/`--only-blocks`

#### Log debug messages: `-d`/`--debug`

#### Force rebuild: `-f`/`--force`

#### Submit build as a job: `--job`

#### Skip existing software: `-k`/`--skip`

#### Log to standard output: `-l`

#### Search path for easyconfig files: `-r`/`--robot`

#### Enable regression test mode: `--regtest`

#### Enable online regression test mode: `--regtest-online`

#### Stop installation after a certain step: `-s`/`--stop`

#### Set strictness level: `--strict`

<a name="software-build-options"/>
## Software build options

These options can be used to specify parameters for the software that EasyBuild will build and install, on the command line.
Using these options requires to specifies a search path for easyconfig files, using `-r`/`--robot`, unless one or more easyconfig files are supplied.

They can be used in two scenarios:
 * _without supplying an easyconfig (`.eb`) file_: In this case, EasyBuild will try to find an easyconfig file, taking into account the specifications given. When the `--try-X` options are used, it will try to generate an easyconfig file if no matching one was found, either from an easyconfig file that matches as closely as possible, or from a generic template `TEMPLATE.eb` (if available).
 * _with supplying an easyconfig (`.eb`) file and using the `--try-X` options_: If an easyconfig file is supplied, the specified build options override the parameters set in this easyconfig, or update them (in the case of list options like `patches`).

**Note:** When EasyBuild is run with the `--try-X` options, it is more likely to run into a failed build. EasyBuild does what it can to fill in the blanks, but might get things wrong. Something that is fairly likely to fail for example when new software is being built like this is the sanity check, because by default EasyBuild checks for non-empty `bin` and `lib` directories in the installation directory.

Using a single `--try-X` command line option is sufficient to enable EasyBuild to generate an easyconfig file is no suited one can be found.

#### Specify name of software to build: `--software-name`, `--try-software-name`

Specify the name of the software to build. 

This is an alternative way of running EasyBuild, without having to supply an easyconfig file.

Examples:

 * install software for which an easyconfig file is available: `eb -r $EBHOME/easybuild/easyconfigs --software-name=bzip2`; 
EasyBuild will pick the best matching available easyconfig file: latest version of software/toolkit, etc. It will not choose between multiple available toolkits.

 * install new software that is unknown to EasyBuild: `eb -r $EBHOME/easybuild/easyconfigs --software-name=unknown-software`; 
EasyBuild will generate an easyconfig file based on a template (if available), and will try to build the software using that generated easyconfig (although this is likely to fail)

#### Specify version of software to build: `--software-version`, `--try-software-version`

Specify the version of the software to be built.

Examples:

 * try to find an easyconfig file for building bzip2 v1.0.6:
```bash
eb -r $EBHOME/easybuild/easyconfigs --software-name bzip2 --software-version=1.0.6
```

 * try to find or generate an easyconfig file for building bzip2 v1.0.8:
```bash
eb -r $EBHOME/easybuild/easyconfigs --software-name bzip2 --try-software-version=1.0.8
```
The closest matching (less recent) version will be selected as a reference point if an easyconfig file needs to be generated.

 * generate an easyconfig file for building bzip2 v1.0.8 based on the given easyconfig file:
```bash
eb bzip2-1.0.6-ictce-4.0.6.eb --try-software-version=1.0.8
```

#### Specify name of toolkit to build with: `--toolkit-name`, `--try-toolkit-name`

Specify the name of the toolkit to build with.

This option is required if EasyBuild is able to find easyconfig files that support building the specified software with multiple toolkits; EasyBuild doesn't pick between multiple available toolkits with different names.

Example:

```bash
eb --robot $EBHOME/easybuild/easyconfigs --software-name=bzip2 --toolkit-name=ictce
```

#### Specify version of toolkit to build with: `--toolkit-version`, `--try-toolkit-version`

Specify the version of the toolkit to build with.

If this option is not specified, EasyBuild will pick the last version of the specified or single available toolkit.

Examples:

```bash
eb bzip2-1.0.6-ictce-4.0.6.eb --try-toolkit-name goalf --try-toolkit-version=1.1.0-no-OFED
eb --software-name=bzip2 -r $EBHOME/easybuild/easyconfigs --toolkit-version=4.0.6 # only easyconfig for ictce available
```

#### Specify the toolkit to build with (name and version): `--toolkit`, `--try-toolkit`

Specify name and version of the toolkit to build with.

Syntax: `--toolkit name,version`

Example:

```bash
eb -r easybuild/easyconfigs --software-name bzip2 --toolkit=ictce,4.0.6
eb bzip2-1.0.6-ictce-4.0.6.eb --try-toolkit goalf,1.1.0-no-OFED
```

#### Specify additional build parameters: `--amend`, `--try-amend`

Additional build specifications can be supplied via the `--amend`/`--try-amend` parameters, which can be specified multiple times.

Syntax: `--amend parameter=value[,value]`

Examples:

```bash
# specifying a desired version prefix
eb -r $EBHOME/easybuild/easyconfigs --software-name bzip2 --try-amend versionprefix=alpha
# specifying a version suffix to use
eb bzip2-1.0.6-goalf-1.1.0-no-OFED.eb --software-version=1.0.4 --try-amend versionsuffix=-old-and-buggy
# specifying URL for downloading source for currently unknown software
eb -r $EBHOME/easybuild/easyconfigs --software-name=tar --toolkit goalf,1.1.0-no-OFED \
--software-version 1.26 --try-amend=sourceURLs=http://ftpmirror.gnu.org/tar,
# specify update for list of patches
eb bzip2-1.0.6-goalf-1.1.0-no-OFED.eb --try-software-version=1.0.4 --try-amend patches=fix-1.0.4.patch,backport_fixes_1.0.5.patch
```

<a name="override-options"/>
## Override options

#### EasyBuild configuration file: `-C`/`--config`

#### Specify easyblock/class to build with: `-e`/`--easyblock`

#### Peform installation in a test directory: `-p`/`-pretend`

#### Skip testing: `-t`/`--skip-tests`

<a name="informative-options"/>
## Informative options

#### Show available easyconfig parameters: `-a`/`--avail-easyconfig-params`

#### Show list of available classes/easyblocks: `--dump-classes`

#### Search for module files in robot directory: `--search`

#### Show EasyBuild version: `-v`/`--version`

####  Create dependency graph: `--dep-graph`
