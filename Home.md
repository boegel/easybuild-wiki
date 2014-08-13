**[[EasyBuild]]** is a software build and installation framework in Python that allows you to install software in a structured, repeatable and robust way.

## Documentation

These wiki pages contain as the EasyBuild documentation.

**NOTE: The wiki pages are in need of a serious update, we're working on it.**


### For EasyBuild users

* [[What is EasyBuild?|EasyBuild]] (updated)
* [[License]]
* [[Dependencies]] (updated)
  * [[Installing environment modules without root permissions]] (up-to-date)
  * [[Installing Lmod without root permissions]] (up-to-date)
* [[Installing EasyBuild]] (updated)
 * [[Bootstrapping EasyBuild]] (ok, up-to-date)
* [[Setting up tab completion for bash]] (ok, up-to-date)
* [[Quick demo for the impatient]] (updated)
* [[Getting started]] (ok, but need update to new style configuration)
 * [[Configuration]] (updated)
* [[Using EasyBuild]]
 * [[Automatic dependency resolution]]
 * [[OS-specific notes]]
* [[Step-by-step guide]]
* [[Compiler toolchains]] (up-to-date)
* [[Easyconfig files]] (updated, may need work)
 * [[Examples]]
* [[List of supported software packages]] (updated) (legacy list [here](https://github.com/hpcugent/easybuild/wiki/List-of-supported-software-packages/ede46976d7367a86fe76ae79adba7b8e9fd9f118))
* [[Conference calls]]
* [[FAQ]] (updated, may need additional entries)
* [[Contact]]
* [[Other tools]]

### For EasyBuild developers

* [[Development guide]]
* [[Deprecated functionality]]
* API documentation (framework, easyblocks): [stable (master)](https://jenkins1.ugent.be/job/easybuild-framework_unit-test_hpcugent_master/Documentation/?) - [develop](https://jenkins1.ugent.be/job/easybuild-framework_unit-test_hpcugent_develop/Documentation) (automatically updated)
* [[Setting up your own easyblocks repository]] (ok, up-to-date)
* [[Encode class names]]
* [[Tutorial: building WRF after adding support for it]] (ok, up-to-date)
* [[Packaging and versioning]]
* [[Unit and regression testing]] (ok, up-to-date)
* [[Release process]] (ok, up-to-date)
* [[Release schedule]] (ok, up-to-date)
* [[Using a custom module naming scheme]] (ok, up-to-date)
* [[Contributing back]]
* [[Review process for contributions]]
* [[Code style]]
* [[Contributing to the EasyBuild documentation wiki]] 
* [[Experimental repo]]

## Presentations

* April 2012 @ [HEPIX spring workshop 2012](https://indico.cern.ch/contributionDisplay.py?sessionId=3&contribId=39&confId=160737): _EasyBuild: building software with ease_, by Jens Timmerman ([slides (PDF)](http://hpc.ugent.be/easybuild/easybuild_hepix_spring_2012.pdf), EasyBuild v.0.5)
* November 2012 @ [PyHPC-2012 workshop at Supercomputer 2012 conference](http://sc12.supercomputing.org/schedule/event_detail.php?evid=wksp118): _EasyBuild: Building Software With Ease_, by Kenneth Hoste, Jens Timmerman, Andy Georges and Stijn Deweirdt ([paper](http://hpcugent.github.com/easybuild/files/easybuild-PyHPC-SC12_paper.pdf), [slides full talk (PDF)](http://hpcugent.github.com/easybuild/files/easybuild-PyHPC-SC12_slides.pdf), EasyBuild v1.0)
* February 2013 @ [FOSDEM'13](http://fosdem.org/2013/): EasyBuild lightning talks in _FOSS for Scientists_ and _Python_ devrooms (EasyBuild v1.1)
* March 2013 @ [[3rd EasyBuild hackathon]]: _EasyBuild: Building Software With Ease_, by Kenneth Hoste ([slides (PDF)](http://hpcugent.github.com/easybuild/files/easybuild_hackathon_Cyprus_20130311.pdf), EasyBuild v1.2)
* June 2013 @ [ISC'13](http://www.isc-events.com/isc13/): BoF session: _Best Practices in Building & Installing Scientific Software_ ([slides (PDF)](http://hpcugent.github.io/easybuild/files/easybuild_BoF_ISC13_20130619.pdf), EasyBuild v1.5.0)
* October 2013 @ [[4th EasyBuild hackathon]]: _Introduction to EasyBuild_, by Kenneth Hoste ([slides (PDF)](http://hpcugent.github.io/easybuild/files/EasyBuild_introduction_hackathon-Cyprus-Oct13.pdf), EasyBuild v1.8.2)
* November 2013 @ [[SC13 BoF session]]: _EasyBuild lightning talk_, by Andy Georges ([slides (PDF)](http://hpcugent.github.io/easybuild/files/SC13_BoF_EasyBuild.pdf), EasyBuild v1.9.0)

## Videos

 * [Introduction to EasyBuild](http://www.youtube.com/watch?v=bOeNsfLB2t4) (Oct'13, EasyBuild v1.8.2)
 * [WRF example use case](http://www.youtube.com/watch?v=e7fyHtO8_qs) (Oct'13, EasyBuild v1.8.2)
 * [EasyBuild status update](http://www.youtube.com/watch?v=A140WvbqaNw) (Oct'13, EasyBuild v1.8.2)

## Meetings

### EasyBuild hackathons

* [[1st EasyBuild hackathon]]: Aug. 16th-17th 2012, Ghent (Belgium)
* [[2nd EasyBuild hackathon]]: Nov. 28th-29th 2012, Kirchberg (Luxembourg)
* [[3rd EasyBuild hackathon]]: Mar. 11-13th 2013, Nicosia (Cyprus)
 * quick links: [EasyBuild presentation](http://hpcugent.github.com/easybuild/files/easybuild_hackathon_Cyprus_20130311.pdf) - [CUDA presentation (NVIDIA)](http://hpcugent.github.com/easybuild/files/CUDA_Toolkit_for_Sysadmins.pdf) - [GPGPU application notes (NVIDIA)](https://github.com/hpcugent/easybuild/wiki/GPGPU-apps-notes-NVIDIA)
 * notes: [day 1](https://github.com/hpcugent/easybuild/wiki/3rd-easybuild-hackathon---meeting-minutes-day-1) - [day 2](https://github.com/hpcugent/easybuild/wiki/3rd-easybuild-hackathon---meeting-minutes-day-2) - [day 3](https://github.com/hpcugent/easybuild/wiki/3rd-easybuild-hackathon---meeting-minutes-day-3)
* [[4th EasyBuild hackathon]]: Oct. 22nd-24th 2013, Nicosia (Cyprus)
 * quick links: [UNITE presentation](http://hpcugent.github.io/easybuild/files/EasyBuild_hackathon_Cyprus_Oct13_UNITE.pdf) - [EasyBuild introduction](http://hpcugent.github.io/easybuild/files/EasyBuild_introduction_hackathon-Cyprus-Oct13.pdf) - [EasyBuild status update](http://hpcugent.github.io/easybuild/files/EasyBuild_status-update_hackathon-Cyprus-Oct13.pdf)
 * notes: [day 1](https://github.com/hpcugent/easybuild/wiki/4th-easybuild-hackathon---meeting-minutes-day-1) - [day 2](https://github.com/hpcugent/easybuild/wiki/4th-easybuild-hackathon---meeting-minutes-day-2) - [day 3](https://github.com/hpcugent/easybuild/wiki/4th-easybuild-hackathon---meeting-minutes-day-3)
  * recorded presentations: [Introduction to EasyBuild](http://www.youtube.com/watch?v=bOeNsfLB2t4) - [WRF example use case](http://www.youtube.com/watch?v=e7fyHtO8_qs) - [EasyBuild status update](http://www.youtube.com/watch?v=A140WvbqaNw)
* [[5th EasyBuild hackathon]]: Feb. 19th-21st 2014, Jülich (Germany)
 * [EasyBuild introduction slides](http://users.ugent.be/~kehoste/EasyBuild_introduction_hackathon-JSC-Feb14.pdf)
 * notes: [day 1](https://github.com/hpcugent/easybuild/wiki/5th-easybuild-hackathon---meeting-minutes-day-1) - [day 2](https://github.com/hpcugent/easybuild/wiki/5th-easybuild-hackathon---meeting-minutes-day-2) - [day 3](https://github.com/hpcugent/easybuild/wiki/5th-easybuild-hackathon---meeting-minutes-day-3)
* [[6th EasyBuild hackathon]]: June 18-20th 2014, Vienna (Austria)

### Conferences and meetings

 * [[Supercomputing'2012 / PyHPC|Notes on EasyBuild promotion at Supercomputing'2012]]: EasyBuild was actively promoted at Supercomputing'2012 in Salt Lake City during Birds-of-a-Feather sessions, the PyHPC workshop and the exhibit
 * FOSDEM'13: EasyBuild lightning talks in FOSS for Scientists and Python devrooms, by Jens Timmerman and Kenneth Hoste (resp.)
 * ISC'13: [BoF session "Best Practices in Building & Installing Scientific Software"](http://www.isc-events.com/isc13_ap/presentationdetails.php?t=contribution&o=2108&a=select&ra=eventdetails)
  * [slides (PDF)](http://hpcugent.github.com/easybuild/files/easybuild_BoF_ISC13_20130619.pdf)
 * [[SC13 BoF session]]: "Getting Scientific Software Installed: Tools and Best Practices"
 * [FOSDEM'14](https://fosdem.org/2014/): "HPC and computational science" devroom (more info soon)
 * [[ISC'14 BoF session]]: "Getting Scientific Software Installed: Tools and Best Practices"

### Conference calls

 * [[20121126 HashDist|Notes-on-EasyBuild-HashDist-conf-call-(20121126)]]: Google hangout conf call with Dag Sverre Seljebotn on HashDist and EasyBuild
 * [bi-weekly EasyBuild conf calls](Conference calls)