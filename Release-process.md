This page describes the steps to be taken to release a new version of EasyBuild.

Do make sure you have an account on PyPi, see [her](http://pypi.python.org/pypi?%3Aaction=register_form).

### Step 0: Make sure EasyBuild is release-ready

To make sure that EasyBuild is ready to release, use the ```easybuild/scripts/prep_for_release.py``` script on each of the EasyBuild packages, as follows:

* _easybuild-framework_: ```python easybuild/scripts/prep_for_release.py```
* _easybuild-easyblocks_: ```python ../easybuild-framework/easybuild/scripts/prep_for_release.py easybuild/easyblocks/__init__.py```
* _easybuild-easyconfigs_: ```python ../easybuild-framework/easybuild/scripts/prep_for_release.py setup.py ```

Next to running the `prep_for_release.py` script, the installed EasyBuild packages should be able to pass a full regression test.

If the output of this script doesn't report any serious issues, and a full regression test has passed, EasyBuild should be ready to release.

### Step 1: Publish new release on PyPi

The following two steps should be repeated for each of the three EasyBuild subpackages (_easybuild-framework_, _easybuild-easyblocks_, _easybuild-easyconfigs_), as well as for the meta-package _easybuild_.

**Note: make sure do perform the steps below in a pristine copy, i.e. not a working copy that may contain unfinished/untracked work!**

#### Step 1.1: Register new version on PyPi

First, make sure the new version of an EasyBuild package gets registered on the Python Package Index (PyPi):

```bash
python setup.py register
```

The first time, you'll need to enter your PyPi login information, which will then be cached to ```$HOME/.pypirc```.

#### Step 1.2: Upload new version to PyPi

To upload the new version of an EasyBuild package to PyPi, use:

```bash
python setup.py sdist upload
```

### Step 2: Tag released version in git

You should also tag the version in git for each of the EasyBuild repositories, and push the tag to GitHub (replace ```1.0``` with the actual version number in the commands below):

```bash
git tag -a v1.0
git push origin v1.0
```

### Step 3: Retest bootstrap script

```bash
curl -O https://raw.githubusercontent.com/hpcugent/easybuild-framework/develop/easybuild/scripts/bootstrap_eb.py
python bootstrap_eb.py $HOME/.local/easybuild
```

#### Step 3.1: Install EasyBuild with a bootstrapped EasyBuild

Try to install (an old) EasyBuild with bootstrapped EasyBuild module (should work since EasyBuild v1.8.2).

```bash
export MODULEPATH=$HOME/.local/easybuild/modules/all
module load EasyBuild
eb EasyBuild-1.0.0.eb --force
```

### Step 3: Announce release

Finally, announce the newly released version through the various channels, i.e.:

* website http://hpcugent.github.com/easybuild
* mailing list easybuild@lists.ugent.be
* Twitter account [@easy_build](http://twitter.com/easy_build)
* [Google+ page](https://plus.google.com/b/116140073126217770418/116140073126217770418/posts)
* IRC channel #easybuild

### Step 4: Update list of supported software

See [[List of supported software packages]].

### Step 5: Update release schedule

See [[Release schedule]] .