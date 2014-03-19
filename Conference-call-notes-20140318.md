(back to [[Conference calls]])

Notes on the 9th EasyBuild conference call, Tuesday Mar. 18th 2014 (3pm - 3.30pm CET)

 * follow link in "Video calls" section on [EasyBuild community Google+ page](https://plus.google.com/communities/103632287931200436158)

#### Attendees

Alphabetical list of attendees (5):

* Petar Forai (GMI, Austria)
* Fotis Georgatos (University of Luxembourg)
* Kenneth Hoste (HPC-UGent, Belgium)
* Alan O'Cais (JSC, Germany)
* Ward Poelmans (UGent, Belgium)

#### Agenda

* permission issues with multiple EasyBuild users (Alan)

#### Notes

##### Permission issues with multiple EasyBuild users

* context: multiple EasyBuild users sharing an install target
* even with use of `umask`, permission issues on software/modules directory trees exist
* EasyBuild also fiddles with permissions, partially undoes permissions set based o `umask`
  * see use of `adjust_permissions` in `post_install_step` method in `easyblock.py`
  * parent directories of install dirs are created via `make_dir` method, which follows `umask` settings (?)
* dedicated issue opened for follow-up on this: https://github.com/hpcugent/easybuild-framework/issues/885

##### Tour of EasyBuild source code

* suggestion by Petar: short tour around EasyBuild framework source code by Kenneth
  * during a next conf call
* Kenneth: needs to be prepared well, next conf call might be difficult
  * around same time of release of EasyBuild v1.12, so busy times
  * week before one week off work for Kenneth, so likely crazy busy
* maybe even a dedicated conf call?

##### Problems with installing Python packages on SuSE systems

* install EasyBuild with EasyBuild on SLES failed
* `--prefix` using in `setup.py install` command is simply ignored
* installation target is somehow redefined to a system location, thus failing
* workaround is to put a `.pydistutils.cfg` file in `$HOME` specifying an empty prefix
* dedicated issue opened for follow-up on this: https://github.com/hpcugent/easybuild-easyblocks/issues/373