Using EasyBuild in the simplest form is really easy: just run the `easybuild.sh` script and supply a `.eb` easyconfig file to it, and EasyBuild will try and build/install the software as specified in the `.eb` file.

**Note**: we will use `$EBHOME` to indicate the location where EasyBuild was installed.

```
$EBHOME/easybuild.sh example.eb
```

EasyBuild offers various command line options to access other features, see below.

## `-h`, `--help`: List of available options

To obtain the list of available options, just run the `easybuild.sh` script with the `-h` or `--help` option, which will provide output as shown below. Running the script without specifying a `.eb` file will also generate this output.

The _usage_ description at the top specifies `build.py` because `easybuild.sh` is just a wrapper around `easybuild/build.py`, which updates the `PYTHONPATH` to ensure that EasyBuild can work as expected.

```
Usage: build.py [options] easyconfig [..]

Builds software package based on easyconfig (or parse a directory)
Provide one or more easyconfigs or directories, use -h or --help more
information.

Options:
  -h, --help            show this help message and exit
  -C CONFIG, --config=CONFIG
                        path to EasyBuild config file [default:
                        $EASYBUILDCONFIG or easybuild/easybuild_config.py]
  -r path, --robot=path
                        path to search for easyconfigs for missing
                        dependencies
  -a, --avail-easyconfig-params
                        show available easyconfig parameters
  --dump-classes        show classes available
  --search=SEARCH       search for module-files in the robot-directory
  -e easyblock.class, --easyblock=easyblock.class
                        loads the class from module to process the spec file
                        or dump the options for [default: Application class]
  -p, --pretend         does the build/installation in a test directory
                        located in $HOME/easybuildinstall
  -s STOP, --stop=STOP  stop the installation after certain step(valid: cfg,
                        source, patch, configure, make, install, test,
                        postproc, cleanup, packages)
  -b blocks, --only-blocks=blocks
                        Only build blocks blk[,blk2]
  -k, --skip            skip existing software (useful for installing
                        additional packages)
  -t, --skip-tests      skip testing
  -f, --force           force to rebuild software even if it's already
                        installed (i.e. can be found as module)
  -l                    log to stdout
  -d, --debug           log debug messages
  -v, --version         show version
```

## `-C`, `--CONFIG`: Configuration file
## `-r`, `--robot`: Automatic dependency resolution (_robot_)
## `-a, --avail-easyconfig-params`: List of available easyconfig parameters
## `--dumps-classes`: 
## `--search`: 
## `-e`, `--easyblock`: 
## `-p`, `--pretend`: 
## `-s`, `--stop`: 
## `-b`, `--only-blocks`: 
## `-k`, `--skip`: 
## `-t`, `--skip-tests`: 
## `-f`, `--force`: 
## `-l`: 
## `-d`, `--debug`:
## `-v`, `--version`:
