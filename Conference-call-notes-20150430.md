(back to [[Conference calls]])

Notes on the 28th EasyBuild conference call, Thursday April 30th 2015 (3.00pm - 3.30pm CET)

#### Attendees

Alphabetical list of attendees (5):

* Petar Forai (IMP/IMBA, Austria)
* Eric Gregory (JSC, Germany)
* Kenneth Hoste (HPC-UGent, Belgium)
* Robert McLay (TACC, US)
* Robert Schmidt (OHRI, Canada)

#### Agenda

* EasyBuild 2.1 (Kenneth)
* EasyBuild on Cray (Kenneth/Petar)
  * see https://github.com/hpcugent/easybuild/wiki/EasyBuild-on-Cray
* support for module properties (Kenneth)

#### Notes

##### EasyBuild 2.1

* documentation is already updated for v2.1, release is going to happen anytime now (certainly today)
* short overview of major features added in v2.1, see http://easybuild.readthedocs.org/en/latest/Release_notes.html

##### EasyBuild on Cray

* report on current status @ https://github.com/hpcugent/easybuild/wiki/EasyBuild-on-Cray
* next hackathon in Espoo (May 4-5 2015) is largely Cray-themed

##### Support for module properties

* properties: namespace/values (dict)
* define known properties in EasyBuild config, tell Lmod about them by generating another $LMODRC file
* keywords (non-Lmod specific), whatis
* some properties will be in easyconfig (e.g. arch, moduleclass), others will 

##### Packaging support

* should work now, needs docs and feedback
* get in for EB v2.2 under --experimental
* support for installing FPM with EB is a prerequisite
* way in which dependencies are handled may require more flexibility to let people do what they want
* package per EB installation vs multiple EB installations in a single RPM (e.g. Szip+HDF5+netCDF @ TACC)
  * let Bundle easyblock do multiple installs by calling out to different easyblocks, then create package + module?
* TACC as a prime tester? keep under `--experimental` until after hackathon in Austin?
  * already requests received to support EB @ TACC

##### DB support for tracking module usage in Lmod

* via a dedicated hook
* + accompanying analysis tools
