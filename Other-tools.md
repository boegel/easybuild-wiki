A list of other software tools that kind of look like EasyBuild

 * [Rocks](http://git.rocksclusters.org/cgi-bin/gitweb.cgi): collection of Makefiles used to build software for the Triton cluster at UCSD
 * [waf](http://code.google.com/p/waf/): Waf is a Python-based framework for configuring, compiling and installing applications.
 * [A-A-P](http://www.a-a-p.org/) (completely outdated) A-A-P makes it easy to locate, download, build and install software. It also supports browsing source code, developing programs, managing different versions and distribution of software and documentation. This means that A-A-P is useful both for users and for developers.

 * [TWW](http://www.thewrittenword.com): Company that builds applications from source and ships them as rpm.
 * [oscar](http://svn.oscar.openclustergroup.org/trac/oscar) OSCAR allows users, regardless of their experience level with a *nix environment, to install a  Beowulf type high performance computing cluster. It also contains everything needed to administer and program this type of HPC cluster. OSCAR's flexible package management system has a rich set of pre-packaged applications and utilities which means you can get up and running without laboriously installing and configuring complex cluster administration and communication packages. It also lets administrators create customized packages for any kind of distributed application or utility, and to distribute those packages from an online package repository, either on or off site.
 * [HomeBrew](http://mxcl.github.com/homebrew/): Package system for OSX, that installs (unix) packages in their own containers, tracking dependencies. Written in Ruby, currently hard-dependent on OSX.
 * [pkgsrc](http://www.pkgsrc.org/): Package manager for [NetBSD](http://www.netbsd.org) which is user installable and runs on Linux. It's very good for users and developers who work on shared systems and don't have privileged access.
 * [Nix](http://nixos.org/nixpkgs/): A purely functional package manager, it install (unix) packages in a dedicated prefixed with a hash in the path, such that different builds/versions with different dependencies can be installed without affecting each other. NixOS is a Linux distribution based on Nix.
 * [Guix](http://www.gnu.org/software/guix/): GNU Guix is a purely functional package manager, and is based on the Nix package manager. It provides Guile Scheme APIs, including high-level embedded domain-specific languages (EDSLs), to describe how packages are to be built and composed.
 * [Paludis](http://paludis.exherbo.org/): a multi-format package manager used by Gentoo (and others), improved version of Portage
 * [SWTools](http://users.nccs.gov/~fm9/SWTools-Manual-1.0.pdf): SWTools is python code combined with a directory hierarchy and rules to create an infrastructure for software management; or SoftWare Tools; is used by Oak Ridge National Labs for building/installing scientific software (e.g. on Titan)
 * [others (Wikipedia)](http://en.wikipedia.org/wiki/List_of_build_automation_software)

## Papers

 * [Why Johny can't build](http://www.grosskurth.ca/bib/2003/dubois.pdf) The “build problem” is the task of creating a system that delivers a software system on a wide variety of platforms that have various programming tools and configurations.
 * [Enhancing Portability of HPC Applications across High-end Computing Platforms](http://www.cecs.uci.edu/~papers/ipdps07/pdfs/HCW-1569012649-paper-1.pdf)