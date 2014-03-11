(back to [[Conference calls]])

Notes on the 7th EasyBuild conference call, Tuesday Feb. 18th 2014 (3pm -3.30pm CET)

 * follow link in "Video calls" section on [EasyBuild community Google+ page](https://plus.google.com/communities/103632287931200436158)

#### Attendees

Alphabetical list of attendees (2):

* Kenneth Hoste (HPC-UGent, Belgium)
* Geert Jan Bex (UHasselt (VSC partner), Belgium)

#### Agenda

* ~~update on support for easyconfig format v2 (Kenneth)~~ **skipped**
* tips & best practices for dealing with errors in a non-trivial build process (question by Geert Jan)

#### Notes

##### Tips & best practices for dealing with errors in a non-trivial build process

* dive into the (debug) build log
 * start from the end, look for `run_cmd` entries or `Running method * part of step *` and any error messages that occur in the corresponding output
 * we should look into splitting the log into separate log files, one per step, to make debugging problems significantly easier
 * having a `devel` log level, i.e. one level more verbose than `debug`, might be useful too; this can help in making debug logs more readable, by avoiding lots of output that was only relevant when developing new features (e.g. toolchain processing, setting up the environment, etc.)

* setting up an environment like the one in which EasyBuild operates
 * stopping after a particular step with `-s` or `--stop` is a first (small) step
 * to set up the actual environment, the devel modules should be used (but they're not usable yet/anymore in current releases, cfr. https://github.com/hpcugent/easybuild-framework/issues/109)
 * we should generate separate devel modules per step, in the build directory (see also https://github.com/hpcugent/easybuild-framework/issues/217)

* side question: uninstall command line option (cfr. https://github.com/hpcugent/easybuild-framework/issues/590)
 * uninstalling software can be easily done naively by removing the install directory and the corresponding module
 * this doesn't take into account dependencies though; the software being removed may be a dependency to others
  * for this, reverse dependency tracking needs to be implemented