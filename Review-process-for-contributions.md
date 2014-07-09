Contributions to EasyBuild, made via [pull requests](Contributing back), are thoroughly reviewed and evaluated before they are merged in to the `develop` branch, ready for being part of the next release of EasyBuild.

This process is important to **ensure stability of the `develop` branches, and high quality and timely releases**.

## Review process details

Reviews mainly consist of evaluating the technical aspect of contributions, which includes **functionality** (_what is being contributed?_) and **code** (_how was it implemented?_). Another aspect that is consider is **style** (_how does the code look?_).

The following specific attention points are taken into account when contributions are reviewed:

 * implementation
 * unit test suite
 * code style
 * testing result

Depending on the particular EasyBuild repository (framework, easyblocks or easyconfigs) to which the contribution is being made, there is a different focus on each of these.

### Implementation

The implementation of the contribution is critically evaluated. The reviewer is encouraged to issue remarks (by means on inline comments in the pull request) with remarks and suggestions for improvements.

### Unit test suite

For every pull request, our [Jenkins setup](https://jenkins1.ugent.be/view/EasyBuild%20(develop)/) will automatically run the unit test suite for that repository, and report back on the outcome in the pull request itself.

**Only pull requests for which the unit test suite (still) passes, i.e. for which Jenkins gives a green light, are eligible for merging.**

For contributions to the EasyBuild framework, additional unit tests should be added when new functionality is being implemented, or when existing functionality is extended.

### Code style

Contributions are also reviewed on code style, with the intention to maintain a uniform feel across the EasyBuild codebase (which significantly lowers the bar for others).

For Python code, we mainly adhere to the [PEP008](http://www.python.org/dev/peps/pep-0008/) code style, with a couple of exceptions (e.g. max column width is 120 characters).

A couple of noteworthy code style aspects are:

 * correct indentation: **no** tabs, 4-space indentation
 * naming:
  * meaningful variable and function/method names
  * no single letter names (except in list comprehensions)
  * **no** CamelCase (only in class names)
 * docstrings for modules, classes, and functions and methods are considered required
 * code comments for non-trivial **blocks** of code are desirable

**TODO**: put together a script to help with code style review, e.g. based on PyLint (where applicable).

### Testing result

Obviously, the contributed functionality or support should work for other people interested in it.

The person reviewing the contribution is expected to test it as good as possible, and report back on the result in the pull request.

#### Automated testing of easyconfigs pull requests

Since EasyBuild v1.13, the command line options `--from-pr` and `--upload-test-report` are available to easily test easyconfigs pull requests (PRs) on your system.
When `--from-pr` is used, `eb` will:

 1. fetch all patched files from the specified easyconfigs PR on GitHub, i.e. easyconfig files, patches, etc.
 2. build all touched easyconfig files
 3. compose a comprehensive test report (in Markdown format)
     * this includes test results, system information (OS, Python, ...), EasyBuild configuration, session environment, etc.

When `--upload-test-report` is used in addition, `eb` will also:

 * upload the test report as a gist on GitHub
 * upload build logs for failed builds as gists
 * post a comment in the respective pull request on GitHub
     * because of this, a GitHub username and token are required

Notes:

 * you are responsible for making sure that the required versions of `easybuild-framework` and `easybuild-easyblocks` are being used
   * this may be the latest `develop` branches, or an update available in a particular PR (especially w.r.t. easyblocks)
 * you can test an easyconfig PR without reporting back a test result using only `--from-pr` (which doesn't require GitHub credentials)
 * you can generate a test report without uploading it to GitHub using `--dump-test-report`
 * generating a test report also works with other sources of easyconfigs files, e.g. local files, your EasyBuild installation, etc.
 * you should filter the environment dump that is included in the test report using the `-test-report-env-filter` configuration option, e.g. `--test-report-env-filter='^SSH|USER|HOSTNAME|UID|.*COOKIE.*'`

##### Example usage

For example, to test https://github.com/hpcugent/easybuild-easyconfigs/pull/767 (`goolf/1.5.14-no-OFED` toolchain + gzip test case), use:

```
eb --from-pr=767 --github-user=GITHUB_USER --upload-test-report -dfr --test-report-env-filter='^SSH|USER|HOSTNAME|UID|.*COOKIE.*'
```

A couple of remarks:

 * a valid GitHub username must be supplied via `--github-user`
   * a GitHub token for that user must be available in your systems keyring
 * `--robot` (`-r`) is required to make sure the builds are being executed in the right order, i.e. taking interdependencies into account
 * `--force` (`-f`) is required to make sure that modified easyconfig files are being rebuilt if they were built before
 * `--debug` (`-d`) is nice to have, especially w.r.t. failed builds for which build logs are also uploaded
 * filtering the environment dump using `--test-report-env-filter` is advisable

##### Setting things up

To obtain a GitHub token:

 1. visit https://github.com/settings/tokens/new and log in with the GitHub user account you would like to use with EasyBuild
 2. enter a token description, e.g. "EasyBuild easyconfigs PR testing"
 3. make sure (only) the `repo` and `gist` scopes are enabled

To install your GitHub token in your systems keyring:

 1. make sure the [`keyring` Python module](https://pypi.python.org/pypi/keyring) is available for the Python version you're using
    * check with `python -c "import keyring"`
    * note: if you're still using Python 2.4.x, you'll need to use [`keyring` version 0.5.1](https://pypi.python.org/packages/source/k/keyring/keyring-0.5.1.tar.gz); more recent are not compatible with Python versions older than 2.6
 2. add your GitHub token in your keyring using Python:
```
# pro tip: copy-paste this, but do replace GITHUB_USER with your own GitHub username
python -c "import getpass, keyring; keyring.set_password('github_token', 'GITHUB_USER', getpass.getpass())"
```
To check that your GitHub token was installed correctly, use:
```
python -c "import keyring; print keyring.get_password('github_token', 'GITHUB_USER')"
```

**Note**

Depending on your OS configuration and the version of the Python `keyring` module you're using, you may be requested to
use a master password when accessing your keyring. More specific, on desktop systems (GNOME, KDE, OS X, …) the `keyring` module will integrate with your system keyring.

While storing your GitHub token encrypted and password-protected is important, this limits the usefulness of `--upload-test-report` since it implies manual intervention.

If you feel strongly about being able to use `--upload-test-report` fully autonomously you can configure `keyring` to store your GitHub token differently, thus avoiding a master password.

To do so, create the following text file at `$HOME/.local/share/python_keyring/keyringrc.cfg` before you store your GitHub token in your keyring:

```
[backend]
# depending on version of the python-keyring package
# newer versions (supported since python-keyring v1.1)
default-keyring=keyring.backends.file.PlaintextKeyring
# older versions (python-keyring v1.0 or older)
# default-keyring=keyring.backend.UncryptedFileKeyring
```

Note that this does **not** imply that your GitHub token is stored in clear text (it’s base64 encoded), see the file named `keyring_pass.cfg` in `$HOME/.local/share/python_keyring`, although it is definitely less secure to not use a master password.


## Reviewers/testers

Anybody who feels confident enough to review and test a particular contributions can do so.

To help streamline the reviewing and testing process, a couple of EasyBuild community members have agreed with taking up the responsibility of reviewer/tester for the different repositories.

#### Overview

| Name            | GitHub user id | Affiliation   | Test systems                           |
| -------------   |:--------------:| -------------:| --------------------------------------:|
| Stijn De Weirdt | @stdweird      | HPC-UGent     | Scientific Linux 6                     |
| Pablo Escobar   | @pescobar      | UniBAS        | ?                                      |
| Petar Forai     | @pforai        | GMI           | ?                                      |
| Fotis Georgatos | @fgeorgatos    | freelance     | Debian 6, RHEL6                        |
| Kenneth Hoste   | @boegel        | HPC-UGent     | Scientific Linux 6, (OS X, OpenSUSE)   |
| Ward Poelmans   | @wpoely86      | UGent         | Debian 7.2, Fedora 20, SL6             |
| Jens Timmerman  | @JensTimmerman | HPC-UGent     | Scientific Linux 6                     |

#### Assignments
 * [framework](https://github.com/hpcugent/easybuild-framework):
  * Kenneth Hoste (HPC-UGent, _release manager_), @boegel
  * Stijn De Weirdt (HPC-UGent), @stdweird
  * Jens Timmerman (HPC-UGent), @JensTimmerman 
  * Ward Poelmans (UGent), @wpoely86
 * [easyblocks](https://github.com/hpcugent/easybuild-easyblocks):
  * Kenneth Hoste (HPC-UGent, _release manager_), @boegel
  * Jens Timmerman (HPC-UGent), @JensTimmerman
  * Ward Poelmans (UGent), @wpoely86
 * [easyconfigs](https://github.com/hpcugent/easybuild-easyconfigs):
  * Kenneth Hoste (HPC-UGent, _release manager_), @boegel
  * Pablo Escobar (UniBas - SIB), @pescobar
  * Petar Forai (GMI), @pforai
  * Fotis Georgatos (freelancer), @fgeorgatos
  * Ward Poelmans (UGent), @wpoely86

**You do not need to be listed as a voluntary reviewer to help out with reviewing pull requests, either occasionally or just once.**

If you would like to join the list of volunteers for reviewing contributions, please contact Kenneth Hoste.