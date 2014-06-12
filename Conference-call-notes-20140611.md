(back to [[Conference calls]])

Notes on the 13th EasyBuild conference call, Wednesday June 11th 2014 (1.15pm - 1.45pm CET)

 * follow link in "Video calls" section on [EasyBuild community Google+ page](https://plus.google.com/communities/103632287931200436158)

#### Attendees

Alphabetical list of attendees (4):

* Pablo Escobar (UniBas - SIB, Switzerland)
* Petar Forai (GMI, Austria)
* Kenneth Hoste (HPC-UGent, Belgium)
* Bart Verleye (NeSI, New Zealand)

#### Agenda

* discuss deprecated functionality, outlook to EasyBuild v2.0
    * see also https://github.com/hpcugent/easybuild/wiki/Deprecated-functionality

#### Notes

##### General remarks

* some deprecated functionality is only relevant for people who have been using EasyBuild v0.x
* tags/examples should be added for the most important changes
* overall, all changes can be easily taken into account when the deprecated behavior is no longer supported (EasyBuild v2.0)

##### Python compatibility

* Python 2.4 support will be deprecated soon, long term goal is a codebase which is Python 2.6.x/2.7.x and Python 3.x compatible
   * Petar: Python 3 is already the default on Ubuntu, getting Python 2.x to work is a problem

##### EasyBuild configuration

* old-style configuration is deprecated, switching to new-style configuration should be no problem
* see https://github.com/hpcugent/easybuild/wiki/Configuration#legacy-configuration-deprecated

##### Easyconfig parameters

* `software_license` will (maybe) become mandatory
   * should specify which license the software is released under (e.g. GPLv2, BSD, commercial, etc.)
   * goal is to track another imporant aspect of the software we install (next to e.g. homepage, etc.)
   * can be used to decide which sources can be redistributed in the context of an _EasyBuild sources archive_ (which is presently vaporware)
   * `UNKNOWN` may be included as a valid value for `software_license` (which would kind of make it non-mandatory however)
* `makeopts`/`premakeopts` are deprecated, **`buildopts`/`prebuildopts`** should be used
   * only since EasyBuild v1.13.0, so easyconfig files in `easybuild-easyconfigs` repo haven't been updated accordingly yet
   * it should be safe to switch the `buildopts`/`prebuildopts` now, easyblocks/easyconfigs that still use `makeopts`/`premakeopts` will continue working until EasyBuild v2.0 (mapping from `makeopts` to `buildopts` is done internally by `eb`)
   * (post-conf call note: easybuild-easyconfigs unit tests should fail on use of `makeopts` or `premakeopts`)

##### Easyblocks API

* these changes are only relevant for people with their own (non-contributed) custom easyblocks
* `extra_options` static method should now return a value of type `dict` (rather than a list of tuples)
   * this is handled already is existing easyblocks available in the `easybuild-easyblocks` repo, via a `super` call to the parent easyblock; the abstract class `EasyBlock` (part of the framework) will spit out the right return type in the end
      * examples: [[generic MakeCp easyblock|https://github.com/hpcugent/easybuild-easyblocks/tree/master/easybuild/easyblocks/generic/makecp.py]] - [[WRF easyblock easyblock|https://github.com/hpcugent/easybuild-easyblocks/tree/master/easybuild/easyblocks/w/wrf.py]]
* other changes to the easyblocks API are minor, and shouldn't affect end users

##### `easybuild.tools.modules` Python module

* was reorganized in EasyBuild v1.8.0
* internal to EasyBuild, shouldn't affect end users

##### Other changes

* most important other change is moving functions to different modules:
   * `run_cmd`/`run_cmd_qa` to `easybuild.tools.run` (from `*.filetools`)
   * `read_environment` and `modify_env` to `easybuild.tools.environment` (from `*.utilities` and `*.filetools` resp.)
