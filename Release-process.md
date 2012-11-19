This page describes the steps to be taken to release a new version of EasyBuild.

Do make sure you have an account on PyPi, see [her](http://pypi.python.org/pypi?%3Aaction=register_form).

### Step 0: Make sure EasyBuild is release-ready

To make sure that EasyBuild is ready to release, use the ```easybuild/scripts/prep_for_release.py``` script on each of the EasyBuild packages, as follows:

* _easybuild-framework_: ```python easybuild/scripts/prep_for_release.py```
* _easybuild-easyblocks_: ```python ../easybuild-framework/easybuild/scripts/prep_for_release.py easybuild/easyblocks/__init__.py```
* _easybuild-easyconfigs_: ```python ../easybuild-framework/easybuild/scripts/prep_for_release.py setup.py ```

If the output of this script doesn't report any serious issues, EasyBuild should be ready to release.

### Step 1: Register new version on PyPi

First, make sure the new version gets registered on the Python Package Index (PyPi):

```bash
python setup.py register
```

The first time, you'll need to enter your PyPi login information, which will then be cached to ```$HOME/.pypirc```.