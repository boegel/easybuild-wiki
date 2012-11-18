### What is EasyBuild?

EasyBuild is a framework for building and installing software in a structured, repeatable and robust way. EasyBuild is written in Python.

It was developed by the [[High-Performance Computing|http://www.ugent.be/hpc]] team at [[Ghent University|http://www.ugent.be/en]] and was motivated by the need for a system that allows to:

* independently install multiple versions of a software application side-by-side
* support multiple compilers and libraries for building a software application and its dependencies
* keep the build specification simple
* divert from the standard configure / make / make install with custom procedures (which is often necessary for scientific applications)
* use [[environment modules|http://modules.sourceforge.net/]] for dependency resolution and for making the software available to users in a transparent way
* keep a record of the installation logs
* keep track of the build specifications in a version control system

Some key properties of EasyBuild:

* the build specification is described  using a (very concise) easyconfig (an .eb file)
* the [[custom behaviour|DevelopmentGuide]] is described in _easyblocks_; these are Python modules that can be plugged into the EasyBuild framework
* modular support for compilers and accompanying libraries (MPI, linear algebra, etc.)
* fully automated generation of the module files to easily make the software available to users
* the dependencies are resolved using [[environment modules|http://modules.sourceforge.net/]] and can be [[automatically installed|AutomaticDependencyResolution]] using the robot feature
* after the installation, the easyconfigs can be sent to a repository for archiving

For more information on EasyBuild, see the [[documentation wiki on github|Home]].
