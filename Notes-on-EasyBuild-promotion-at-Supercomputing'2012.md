## Overview

* lightning talk at "Python for HPC" BoF session (by JT) ([pic](https://twitter.com/jenstimmerman/status/269234539304480769))
* present EasyBuild in BoF session "Common Practices for Managing Small HPC clusters" (by KH/JT)
 * [slides feeding the discussion](https://sites.google.com/site/smallhpc/)
 * [mailing list](https://groups.google.com/forum/?fromgroups&hl=en#!forum/small-hpc)
* paper presentation at PyHPC workshop (by KH)
 * [paper](http://hpcugent.github.com/easybuild/files/easybuild-PyHPC-SC12_paper.pdf), [slides](http://hpcugent.github.com/easybuild/files/easybuild-PyHPC-SC12_slides.pdf)
* promoting EasyBuild on SC'12 exhibit floor (by JT/KH)
 * list of exhibitors [here](http://iebms.heiexpo.com/iebms/oep/oep_p1_exhibitors.aspx?cc=111112903651&oc=34)

# Notes

## BoF: _Python for HPC_

#### hashdist
* planned project that seems to have _very_ similar goals to EasyBuild
* currently still vaporware, but funding acquired from DoD to work on this
* blog post: http://andy.terrel.us/blog/2012/10/28/python-packaging/
* website: http://hashdist.readthedocs.org/en/latest/
* mailing list: https://groups.google.com/forum/?fromgroups=#!topic/numfocus/JIrVrlcyTq8
* meeting minutes on conf call: https://docs.google.com/document/d/1p59PCdfwES85ohrBh0UM5AJp6JsUYsUYFZlcJ10Fpbk/edit

#### [Monte Lunacek @ University of Colorado](http://www.locazu.com/monte-lunacek-university-colorado-boulder/p1204381) 
* [mlunacek @ GitHub](https://github.com/mlunacek)
* was really interested in ability to build/install things with a single command, will look into it (uses GCC/Intel/PGI compiler)

#### David Brown @ Pacific Northwest National Lab [Environmental Molecular Science Laboratory (EMSL)](http://www.emsl.pnl.gov/)
* https://github.com/EMSL-MSC
* showed great interest, will look into it

#### [Graham Markall @ Imperial College London](http://www.doc.ic.ac.uk/~grm08/) 
* [gmarkall @ GitHub](https://github.com/gmarkall)
* currently installs DOLFIN+deps manually, will be contributing easyblocks for e.g. PyOp2+deps (CUDA, requires CUDA compiler)

#### Adam Deconinck @ Santa Clara/NVIDIA:
* has to administer some NVIDIA clusters (blue gene, cray, dell?)
* played around with v0.8 
* came to thank us
* will likely add support for CUDA compiler

## Exhibit floor

#### University of Tennesee (NICS)
* Daniel Lucio and Robert Whitten
* NICS and NCCS (home of Titan, #1 in Top500) both use '[SWtools]([http://www.olcf.ornl.gov/center-projects/swtools/)', a collection of Python scripts for building/installing software
 * original author: Mark Fahey
 * 'maintainer': Lonnie Crosby
* [http://www.nics.tennessee.edu/computing-resources/kraken/software list of software built with SWtools @ Kraken supercomputer]
* SWtools still requires manual procedure for building+installing
* no longer actively developed, they are happy with it, it works
* doesn't create environment modules automatically, that's still a manual job
* software built on login nodes, workernodes have no access to NFS mount where software is installed
 * login nodes have different architecture than workernodes
 * building ATLAS: performance tests are submitted to workernodes during build, build takes forever
 * software is statically linked, and copied to workernode when job starts by ''aprun'' tool (Cray's mpirun)
* important feature in SWtools: only redo linking step instead of rebuilding from scratch when e.g. MPI library was replaced by Cray
 * old version may be incompatible, e.g. due to changes in drivers
  * so *all* software needs to be relinked, rebuilding from scratch would take forever (only login nodes can be used!)
* another project is being developed @ Oak Ridge?
* contact Robert Whitten, ask for contact at DoE (Tony something)

#### University of Utah:
* need to contact sw installation guy: Wim.Cardoen@utah.edu or Martin Cuma

#### Clemson University
* Galen Collier
* currently rpms, but users can't use these, so interested and  will look into it


#### University of Virginia
* Uvacse does the installations, passed on business card

#### Mississippi State
* do every install by hand, are interested

#### OSC (Ohio State)
* will look Into easybuild

#### King Abdullah University of Science and Technology (KAUST)
* Use blue gene with xlc compiler, but think it is very interesting to share documentation, if it is in form of python implementation this is ok for sure

#### TACC (Texas Austin)
* use RPMs, are happy with this
 * they had paper last year at SC11 about this, by William L. 
* Barth: RPMs work for them, expect the first one Jens brought up (WRF) is a special case
 * they have some people who know how to build this and build this in users home directories separately
 * after explaining how EasyBuild also works for users there was some more interest

#### INRIA
* 7 groups in france, they don't colaborate wrt software installation (yet)
* Jens spoke to someone from Bordeaux, he will pass cards and go to PyHPC workshop on friday

#### NCAR (WRF, WPS, NCL)
* will pass info along to technical guys

#### Purdue
* will pass info to user support software installation team

#### Indiana University
* Scott Michael
* RHEL based
* only two/ three build of an app, retire oldest versions on monthly basis
* collect stats on module usage
* motivation is migrating of software to new systems
* build for common denominator across 3 arch, optimized build on request
* modules are generated manually, build prod is manual
* very interested, will try it out 

#### NCSA (home of Blue Waters)
* got contact (Trish Barker) to ask for user support on how they handle software vuilds
* WRF is one of the 5 sustained petascale apps

#### NASA
* mostly in-house software, some pushed out to communities
* Earth System Grid (ESG) is a nightmare
* same kind of problem as HPC sites
* rely on RPMs for some software stacks

#### Cray
* Kevin Peterson
* no tools for building end-user software
* might be interesting to provide to users
* mail slides, ask for Luis

#### VPAC
* Craig West
* Australian consultancy company for HPC
* interest in EasyBuild
* are working on a "module test" feature to test whether modules are broken

#### Q-Leap
* package EasyBuild for Qluster (Debian-based), rely on it to support commercial software that can't be packaged

#### Virginia Bioinformatics Institute
* interested, will look into it
* use packaging system from Bright Computing

#### Virginia Tec
* very interested,e.g. for building OpenFOAM

#### TU Dresden (M. Kludge)
* no user support guy available, but will be passed

#### South Ural State Univ (Russia)
* very interested, will pass to project manager

#### Stanford
* very interested (3 cards), Sebastian & Gunnar
* suggestion: look into FPM (Ruby), which converts rpm to .deb, and vice versa

#### Rolls Royce (Indianapolis)
* mainly questions:
 * staging of sources with EasyBuild? because workernodes have no access online
 * how many FTEs are requiredfor how many cores/clusters

#### ADMIN magazine
* mentioned EasyBuild to editor, was going to pass it through to article writer

#### David Byte (`SuSe`)
* briefly described EasyBuild, sees orthogonality with `OpenSuSe` build system (e.g. make EasyBuild produce .spec files or RPMs for it)

#### `Open|Speedshop`
* have trouble delivering dependencies to end users, so `EasyBuild` looks interesting

#### Microsoft
* there is nothing that does what module commands does
* but if we can compile it on windows we should just add a `modulecmd windowsshell` and this should work

## Student Cluster Competition competing teams

#### Univ of Texas
* big issues with CAM app

#### Team Venus
* no prior knowledge of Linux, spent 2-3 weeks to build a single app
* EasyBuild would be a great help
* looking for paid internships (7 months)

### Texas Tech
* competing again next year, will check it out

#### Purdue SCC
* building CAM/CESM is a huge pain, but they've figured it out now
* guy who does the CAM is familiar with Python, asked him to contribute back

## PyHPC
* **note**: missing in presentation: cloud of supported software

#### X
* switching easily between shared and static library builds
* partial rebuilds, e.g. only one library, how would you do that?

#### David Wade (General Atomics):
* do you work together with commercial companies e.g. Bright Computing w.r.t. supporting software builds
* they claim to help users out with this, they might want to use EasyBuild and contribute back?
* interested in support for building OpenFOAM (used in a Linux-Windows setup, communication between two OSs via shared FS)
* what's being used for `run_cmd_qa`?
* Sundials, diff eq solver, used by NASA

#### [William Spotz (Sandia National Labs)](http://est.sandia.gov/staff/william.html)
* guy behind Trilinos:
* issue with using GCC on OS X: no gfortran provided, mixed language software requires same GCC compiler versions, which is a pain on OS X
* can EasyBuild offer a solution for that? yes.

#### Andy Terrel (TACC)
* any way to figure out what EasyBuild supports, e.g. for doing partially-featured software builds (e.g. DOLFIN)
* make EasyBuild generate pkgconfig files (*.pc), so DOLFIN can rely on those to figure out what compiler flags were being used, etc.
* very good remark: Windows does matter, in same way as OS X does: people use it on their laptop all the time, and want to build and use the software there as well

#### Scott Rostrup (Univ. of Toronto)
* interested in EasyBuild, will spread the word in Toronto/Canada

#### guy from Norway
* interested in EasyBuild for using it, just switched to lmod, went quite ok
* is interested to contribute support for lmod (put him in contact with other guy (David?))

#### licensing discussion
* GPL is a big issue for companies, and even for some Python hackers
* see [https://github.com/hpcugent/easybuild-framework/issues/335 issue #335]
* A. Terell usually doesn't touch GPL software
* Python is almost always BSD, and it seems like EasyBuild should be as well, so re-license (ASAP)?