(back to [[Conference calls]])

Notes on the 4th EasyBuild conference call, Tuesday Dec. 17th 2013 (3pm -3.30pm CET)

 * follow link in "Video calls" section on [EasyBuild community Google+ page](https://plus.google.com/communities/103632287931200436158)

#### Attendees

Alphabetical list of attendees (5):

* Fotis Georgatos (Uni.lu)
* Kenneth Hoste (HPC-UGent)
* Alan O'Cais (JSC)
* Andreas Panteli (CyI)
* Ward Poelmans (Ghent University)
* Thekla Loizou (CyI)

#### Agenda

* update on EasyBuild activities @ Jülich Supercomputer Centre (JSC) (Alan O'Cais)
* support for group of EasyBuild users with a shared install target (Alan O'Cais)
* keeping track of popular module files (Fotis Georgatos)

#### Notes

##### EasyBuild @ JSC (Alan)

* EasyBuild being considered as _the_ build tool for JUROPA4 (which will include some form of accelerators), currently in exploratory stages
* initial EasyBuild training (last week) at JSC went well, except for a couple of hickups
 * cfr. https://github.com/hpcugent/easybuild-framework/issues/787, https://github.com/hpcugent/easybuild-framework/issues/788
* core team is going to look into EasyBuild in the coming weeks, in preparation for the EasyBuild hackathon at JSC (Feb 19th-21st 2013)
* it would be useful to be able to keep track of who did which build
 * (Kenneth) should become very easy once we have the support for site customizations in place, e.g., making every module set a `$CONTACT` environment variable
* Alan is also looking into making the switch to Lmod, together with EasyBuild

##### Sharing an install target with multiple users (Alan)

* some issues when multiple people are using EasyBuild on the same system
 * cfr. https://github.com/hpcugent/easybuild-framework/issues/788, fixed for EB v1.10
* some issues when multiple people are installing software to the same install path
 * this should be resolved with a proper umask setting
 * alternatively, EasyBuild could be made aware that software is installed by a group of people (e.g. an `easybuild` POSIX group), and act accordingly w.r.t. permissions

##### Keeping track of popular module files (Fotis)

* open question on how people are tracking module usage
 * @ JSC: no tracking done for now
 * @ UGent: by modifying the `module` function definition and logging module commands to syslog
* (Kenneth) Lmod has a proper solution for this, with hooks to make things happen when module commands are run

##### Other

* question by Thekla: working with EasyBuild develop branches, for installing latest version of Intel tools
 * see https://github.com/hpcugent/easybuild/wiki/Installing-EasyBuild#wiki-github_devel_install, replace `pip install --user` with `easy_install --prefix /tmp`, or whatever
 * handy script available in framework repository for exactly this, see https://github.com/hpcugent/easybuild-framework/blob/master/easybuild/scripts/install-EasyBuild-develop.sh