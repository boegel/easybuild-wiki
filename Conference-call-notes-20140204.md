(back to [[Conference calls]])

Notes on the 6th EasyBuild conference call, Tuesday Feb. 4th 2014 (3pm -3.30pm CET)

 * follow link in "Video calls" section on [EasyBuild community Google+ page](https://plus.google.com/communities/103632287931200436158)

#### Attendees

Alphabetical list of attendees (7):

* Pablo Escobar (University of Basel, Switserland)
* Fotis Georgatos (Uni.lu, Luxembourg)
* Kenneth Hoste (HPC-UGent, Belgium)
* Bernd Mohr (Jülich Supercomputer Centre, Germany)
* Alan O'Cais (Jülich Supercomputer Centre, Germany)
* Ward Poelmans (Ghent University, Belgium)
* Georges Tsouloupas (Cyprus Institute, Cyprus)

#### Agenda

* update on support for easyconfig format v2 (Kenneth)
* update on use of EasyBuild @ JSC (Alan)

#### Notes

##### Update on support for easyconfig format v2 (Kenneth)

* currently support example: https://github.com/hpcugent/easybuild-framework/tree/develop/test/framework/easyconfigs/v2.0/libpng.eb
 * shows use of supported and default sections, specifying of dependencies
* not working yet: section markers for software version or toolchains, e.g. `[> 1.0]` or `[goolf < 2.0]`
* values in sections are not Python code, so strings are specified without quotes
* top block can still contain Python code, but we will try to avoid that as much as we can, and add more support in the format v2 parser to get rid of anything that can currently only be done with Python code
* for now, the list of supported versions/toolchains in the `SUPPORTED` section needs to be exhaustive and strict
 * in the future, we'll look into supporting ranges and exceptions (e.g. anything but version `1.2.3`)

##### Update on use of EasyBuild @ JSC (Alan)

* use is still fairly limited, but it's working quite well
 * one success story is the installation of Meep (last available build was years old)
  * includes tons of dependencies, making it a complex and time-consuming build with EasyBuild
  * `ictce` build is a bit complex because the `guile` dependency is not built correctly with `ictce`, so that must be built with `goolf`
* JSC-specific toolchains are there (using compilers and MPI library provided by the system)
* main thing left to do is set up the common build environment, e.g. w.r.t. `umask` setting to avoid permissions problems on common directories, etc.
* switching to Lmod as modules tool is still being considered
 * one desired feature is hiding of dependency modules, and only showing actual applications to users

##### NAMD (George)

* EasyBuild NAMD support is available at CyI, but no pull request has been created yet
 * several other people are interested (e.g. Pablo), so PR should be opened even though the code isn't 'clean'

##### Next EasyBuild hackathon (Fotis)

* suggestion by Fotis for next hackathon end of March seems a bit early, especially w.r.t. timing of JSC event in Feb'14
* maybe Vienna would be a nice venue, given it's 'central' location (Belgium/Germany vs Cyprus); Kenneth will talk about this with our Austrian friends