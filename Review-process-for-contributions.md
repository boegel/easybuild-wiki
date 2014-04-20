Contributions to EasyBuild, made via [pull requests](Contributing back), are thoroughly reviewed and evaluated before they are merged in to the `develop` branch, ready for being part of the next release of EasyBuild.

This process is important to **ensure stability of the `develop` branches, and high quality and timely releases**.

## Review process details

Reviews mainly consist of evaluating the technical aspect of contributions, which includes **functionality** (_what is being contributed?_) and **code** (_how was it implemented?_). Another aspect that is consider is **style** (_how does the code look?_).

The following specific attention points are taken into account when contributions are reviewed:

 * unit test suite
 * code style
 * testing result

Depending on the particular EasyBuild repository (framework, easyblocks or easyconfigs) to which the contribution is being made, there is a different focus on each of these.

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

### Testing result

Obviously, the contributed functionality or support should work for other people interested in it.

The person reviewing the contribution is expected to test it as good as possible, and report back on the result in the pull request.

## Reviewers

Anybody who feels confident enough to review a particular contributions can do so.

To help streamline the reviewing process, a couple of EasyBuild community members have agreed with taking up the responsibility of reviewer for the different repositories.

 * [framework](https://github.com/hpcugent/easybuild-framework):
  * Kenneth Hoste (HPC-UGent, _release manager_)
  * Stijn De Weirdt (HPC-UGent)
  * Jens Timmerman (HPC-UGent)
  * Ward Poelmans (UGent)
 * [easyblocks](https://github.com/hpcugent/easybuild-easyblocks):
  * Kenneth Hoste (HPC-UGent, _release manager_)
  * Jens Timmerman (HPC-UGent)
  * Ward Poelmans (UGent)
 * [easyconfigs](https://github.com/hpcugent/easybuild-easyconfigs):
  * Kenneth Hoste (HPC-UGent, _release manager_)
  * Pablo Escobar (UoB - SIB)
  * Petar Forai (GMI)
  * Fotis Georgatos (freelancer)
  * Ward Poelmans (UGent)

If you would like to join the list of volunteers for reviewing contributions, please contact Kenneth Hoste.