This is a concise step-by-step tutorial to using EasyBuild.

Getting started with [[EasyBuild]] is almost trivial: just follow these simple steps and you can start building and deploying software with EasyBuild yourself.

* [Step 0: Install EasyBuild](#step0)
* [Step 1: Configure EasyBuild](#step1)
* [Step 2: Test configuration](#step2)
* [Step 3: Set up compiler toolchain](#step3)
* [Step 4: Build software with toolchain](#step4)
* [Step 5: Load module and use software](#step5)

***


<a name="wiki-step0">
## Step 0: Install EasyBuild

A pretty obvious prerequisite to using EasyBuild is to have it installed; see [[Installing EasyBuild]].

<a name="wiki-step1"/>
## Step 1: Configure EasyBuild

The first step consists of configuring EasyBuild: see [[Configuration]] for full details if you have not done this yet.

It is essential that you make sure to extend the `MODULEPATH` environment variables with `<install_path>/modules/all` before continuing.
This is required for allowing EasyBuild to determine whether builds were already performed, and also to resolve dependencies.



<a name="wiki-step2"/>
## Step 2: Test your configuration

Once you have EasyBuild configured, test your configuration by installing a simple software package, e.g., the open-source software package `gzip`.
This package has no dependencies, so this makes it a great first example.

Follow these steps to build your first software package using EasyBuild.

<a name="wiki-step2.1"/>


### Step 2.1: Create easyconfig file

Create an easyconfig (.eb file) named `gzip.eb` anywhere on your system (for example, in your home directory).  Put the following contents in this file.

```python
name = 'gzip'
version = '1.4'

homepage = 'http://www.gnu.org/software/gzip/'
description = "gzip (GNU zip) is a popular data compression program as a replacement for compress"

# dummy toolchain, rely on system C compiler
toolchain = {'name': 'dummy', 'version': 'dummy'}

# specify that GCC compiler should be used to build gzip
preconfigopts = "CC='gcc'"

# source tarball filename
sources = ['%s-%s.tar.gz'%(name,version)]

# download location for source files
source_urls = ['http://ftpmirror.gnu.org/gzip']

# make sure the gzip and gunzip binaries are available after installation
sanity_check_paths = {
                      'files': ["bin/gunzip", "bin/gzip"],
                      'dirs': []
                     }

# run 'gzip -h' and 'gzip --version' after installation
sanity_check_commands = [True, ('gzip', '--version')]
```

If the `gzip` source tarball that is specified in the .eb file is not yet available in the source path -- which is defined in
the EasyBuild configuration file -- EasyBuild will try to download the `gzip` source tarball and store it in the `g/gzip`
subdirectory under the source path.

Note that the easyconfig is basically just (valid) Python code.

In this first example, we are using a dummy compiler toolchain, and explicitely specify to build gzip with the GCC compiler by setting preconfigure options. Although this works, it is not a typical way of installing software with EasyBuild; see step 3.

### Step 2.2: Install with EasyBuild

Run EasyBuild, and specify the location of the `gzip.eb` file you created in step 2.1:

```bash
eb gzip.eb
```

This will make EasyBuild build and install the `gzip` software using a ```dummy``` toolchain.


<a name="wiki-step3"/>
## Step 3: Set up a compiler toolchain

EasyBuild uses compiler toolchains to build software. These consist a (set of) compiler(s) to build software,
and a set of libraries to provide extra functionality -- MPI support, BLAS and/or LAPACK routines, etc.

'''Note''': text below is not longer correct, dummy toolchain will not use system compilers

In step 2.1, a dummy compiler toolchain was specified. This means that EasyBuild relies on the compilers and libraries
provided by the system for building software packages. While this may be a good fit for you, it is usually not the most
desirable or best scenario. First, you may have licenses for other compilers, that co-exist next to your system compiler.
Second, if the system is updated, the compiler version and its assorted libraries can change due to an upgrade.
This could break dependencies for existing software builds. Third, relying on the system compiler and libraries also makes
reproducing software package builds quite involved or even impossible across different systems.

To avoid these issues, the first time you fire up EasyBuild, it is a good idea to start by constructing your own (preferred)
compiler toolchains. Note that by design, EasyBuild allows building and using several (different) toolchains next to each other.

For more details on how to build your own toolchain, we kindly refer to the [[compiler toolchains wiki page|Compiler toolchains]]
and the [[step-by-step guide|Step-by-step-guide]].


<a name="wiki-step4"/>
## Step 4: Build a software package using a compiler toolchains

Once you have specified and built your own compiler toolchain, it is time to install `gzip` using that toolchain.

Note that EasyBuild will install a completely new build of `gzip` when doing this, without removing or overwriting
the previous build using the dummy toolchain.

The first step involves changing the `gzip.eb` file. Simply adjust the `toolchain` variable in the `gzip.eb` easyconfig,
detailing the name and version of your toolchains. For example:

```python
toolchain = {'name': 'myToolchain', 'version': '1.2.3'}
```

Make sure this matches the actual name and version of the installed toolchains.

Then, rerun the ```eb``` command while supplying it the `gzip.eb` easyconfig file:

```bash
eb gzip.eb
```

<a name="wiki-step5"/>
## Step 5: Load the module, and use the built software

Finally, you can load the module created by EasyBuild to start using the `gzip` version built with your own compiler toolchain:

    module load gzip/1.4-myToolchain-1.2.3


## Congratulations!

Congratulations, you've just mastered the basics of EasyBuild!

At this point, you are ready to start using EasyBuild for building your favorite set of software packages.

For a detailed example of setting up a compiler toolchain and using it, see the [[step-by-step demo]].

More advanced topics include:

* [[EasyBuild command line options|Using EasyBuild]]
* [[Writing your own easyconfigs|Easyconfig files]]
* [[Installing a set of software packages, with automatic dependency resolution|Automatic dependency resolution]]
* [[Adding support for new software packages by writing easyblocks|Development guide]]
