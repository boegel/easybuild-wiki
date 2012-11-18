Getting started with [[EasyBuild]] is almost trivial: just follow these simple steps and you can start building and deploying software with EasyBuild yourself.

* [Step 1: Configure EasyBuild](#step1)
* [Step 2: Test configuration](#step2)
* [Step 3: Set up compiler toolkit](#step3)
* [Step 4: Build software with toolkit](#step4)
* [Step 5: Load module and use software](#step5)

***


<a name="wiki-step1"/>
## Step 1: Configure EasyBuild

The first step consists of configuring EasyBuild: see [[Configuration]] for full details if you have not done this yet.

It is essential that you make sure to extend the `MODULEPATH` environment variables with `<installPath>/modules/all` before continuing. This is required for allowing EasyBuild to resolve dependencies.

<a name="wiki-step2"/>
## Step 2: Test your configuration

Once you have EasyBuild configured, test your configuration by installing a simple software package, e.g., the open-source software package `gzip`. This package has no dependencies, so this makes it a great first example.

Follow these steps to build your first software package using EasyBuild.

<a name="wiki-step2.1"/>
### Step 2.1: Create easyconfig file

Create an easyconfig (.eb file) named `gzip.eb` anywhere on your system (for example, in your home directory).  Put the following contents in this file.

```python
name = 'gzip'
version = '1.4'

homepage = 'http://www.gnu.org/software/gzip/'
description = "gzip (GNU zip) is a popular data compression program as a replacement for compress"

# dummy toolkit, rely on system C compiler
toolkit = {'name': 'dummy', 'version': 'dummy'}

# source tarball filename
sources = ['%s-%s.tar.gz'%(name,version)]

# download location for source files
sourceURLs = ['http://ftpmirror.gnu.org/gzip']

# make sure the gzip and gunzip binaries are available after installation
sanityCheckPaths = {'files': ["bin/gunzip", "bin/gzip"], 'dirs': []}
# run gzip -h after installation
sanityCheckCommand = True
```

If the `gzip` source tarball that is specified in the .eb file is not yet available in the source path -- defined in the EasyBuild configuration -- EasyBuild will try to download the `gzip` source tarball and store it in the `g/gzip` subdirectory under the source path.

Note that the easyconfig is basically just (valid) Python code.

### Step 2.2: Install with EasyBuild

Run EasyBuild, and specify the location of the `gzip.eb` file you created in step 2.1:

    <path>/easybuild/eb gzip.eb

<a name="wiki-step3"/>
## Step 3: Set up a compiler toolkit

EasyBuild uses compiler toolkits to build software. These consist a (set of) compiler(s) to build software, and a set of libraries to provide extra functionality -- MPI support, BLAS and/or LAPACK routines, etc.

In step 2.1, a dummy compiler toolkit was specified. This means that EasyBuild relies on the compilers and libraries provided by the system for building software packages. While this may be a good fit for you, it is not always the most desirable or best scenario. First, you may have licenses for other compilers, that co-exist next to your system compiler. Second, if the system is updated, the compiler version and its assorted libraries can change due to an upgrade. This could break dependencies for existing software builds. Third, relying on the system compiler and libraries also makes reproducing software package builds quite involved or even impossible across different systems.

To avoid these issues, the first time you fire up EasyBuild, it is a good idea to start by constructing your own (preferred) compiler toolkits. Note that by design, the system allows building and using several (different) toolkits next to each other.

For more details on how to build your own toolkit, we kindly refer to the [[compiler toolkits wiki page|Compiler toolkits]] and the [[step-by-step guide|Step-by-step-guide]].


<a name="wiki-step4"/>
## Step 4: Build a software package using a compiler toolkit

Once you have specified and built your own compiler toolkit, it is time to install `gzip` using said toolkit.

Note that EasyBuild will install a completely new build of `gzip` when doing this, without removing or overwriting the previous build using the dummy (system) toolkit.

The first step involves changing the `gzip.eb` file. Simply adjust the `toolkit` variable in the `gzip.eb` easyconfig, detailing the name and version of your toolkit. For example:

```python
toolkit = {'name':'myToolkit','version':'1.2.3'}
```

Make sure this matches the actual name and version of the installed toolkit.

<a name="wiki-step5"/>
## Step 5: Load the module, and use the built software

Finally, you can load the module created by EasyBuild to start using the `gzip` version built with your own compiler toolkit:

    module load gzip/1.4-myToolkit-1.2.3


## Congratulations!

Congratulations, you've just mastered the basics of EasyBuild!

At this point, you are ready to start using EasyBuild for building your favorite set of software packages.

For a detailed example of setting up a compiler toolkit and using it, see the [[step-by-step demo]].

More advanced topics include:

* [[EasyBuild command line options|Using EasyBuild]]
* [[Writing your own easyconfigs|Easyconfig files]]
* [[Installing a set of software packages, with automatic dependency resolution|Automatic dependency resolution]]
* [[Adding support for new software packages by writing easyblocks|Development guide]]
