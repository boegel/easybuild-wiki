**Note: this is a proposal, [feedback or suggestions](http://easybuild.readthedocs.org/en/latest/#getting-help) for improvement are much appreciated!**

This is a policy to follow when issuing a pull request (PR) to the [easybuild-easyconfigs repository](https://github.com/hpcugent/easybuild-easyconfigs/pulls). It is intended to better structure the large amount of pull requests for easyconfig files, and help avoid duplicate work among contributors.

* use a standard format for your PR title: ``[<toolchain>] <software>, ... (<tag>)``
* open a PR (with dummy easyconfigs) as soon as you start working on something, tag it with `(WIP)`
* update the PR when it's ready for review/testing by others, tag it with `(REVIEW)`
* indicate that your PR should be ready to be merged, after review/testing and fixing remarks or problems, by using the `(OK)` tag
* easyconfig files should only be considered production ready when the PR is merged into `develop`

More details in the sections below.

### Use a standard format for your PR title

Using a title for your PRs according to the following standard format makes it easy to recognise at a glance what it is providing. It also enables meaningful PR titles included in the output of `--search` (as soon as it considers open PRs as well).

``[<toolchain_>] <software>, ... (<tag>)``

* since easyconfigs are specific to a particular _toolchain_, clearly indicating the toolchain being used helps to structure the overview of easyconfigs; both the toolchain name and version should be mentioned, enclosing by square brackets;
  * examples: `[intel/2015a]`, `[goolf/1.5.14-no-OFED]`
* the main software package(s) for which easyconfigs are provided, together with their respective version(s), should be listed in the title
  * examples: `Boost 1.55.0`; `Python 2.7.9, OpenSSL 1.0.1k`;
* tagging your pull request with a short/clear label is useful for indicating its current status; 'standard' tags are:
  * `(WIP)` to indicate that the PR is still work-in-progress
  * `(REVIEW)` to invite others to review and test your PR
  * `(OK)` to indicate that the PR should be ready to be merged in (review/testing is done)

Complete examples:
 * `[intel/2015a] Boost 1.55.0 (WIP)`
 * `[goolf/1.5.14-no-OFED] Python 2.7.9, OpenSSL 1.0.1k (OK)`


### Open a PR as soon as you start working on something

As soon as you start working on a (set of) easyconfig file(s), open a [pull request](https://github.com/hpcugent/easybuild-easyconfigs/compare/), and mark it as being a _work in progress_:

* include placeholder easyconfigs (with the correct filename): empty files, partial files (e.g., only containing definitions for `name`, `version`, `toolchain`), or copies of existing easyconfig files for which only the filename has been adjusted
* include the `(WIP)` tag in the title of the pull request

The key point here is to open a pull request **early** rather than late; that way other people who are planning to look into the same easyconfigs you are working on can reap the benefits of your labor, can contact you to help out, or can review/test your work early in the process.

Easyconfigs for multiple software packages can be added in a single PR, but keep it reasonable: maximum 5 different software packages, 10 easyconfig files.

Tag other EasyBuilders that you know are may be interested in your work, using their GitHub account name preceded by `@`. Example: `@boegel`.


### Update the PR when it's ready for review/testing by others

Once your easyconfigs are ready for testing/review, update the pull request by pushing to the corresponding branch in your GitHub easybuild-easyconfigs repository.

#### Unit tests

Make sure that the unit tests run by Jenkins still pass, i.e. that a 'Test PASSed` comment is made by @hpcugentbot.
If Jenkins is reporting failing unit tests, fix those issues first (details on failing tests are available by clicking the red cross next to the last tested commit, see the `Console output` link in the Jenkins interface).

#### Test report

Submit a test report using the `--upload-test-report` eb command line option (see https://github.com/hpcugent/easybuild/wiki/Review-process-for-contributions#automated-testing-of-easyconfigs-pull-requests), as a confirmation that the easyconfigs are working for you.

Once you've submitted a successful test report, change the tag in the PR title to `(REVIEW)`.
Preferably, add a comment as well so that followers are notified via e-mail, mentioning something like `ready for review/testing`.

#### Review/test process

Work together with the reviewer/tester to get the PR ready for merging, by pushing additional commits into the branch/PR.


### Indicate that your pull request should be ready to be merged in

Once your PR has been reviewed and successfully tested by someone else, change the tag in the pull request title to `(OK)`.

Add a comment addressing the EasyBuild release manager to indicate that the PR should be ready to be merged in: `@boegel: ready to merge in`.


### Production-ready

Once the PR is merged into develop, it can be consider "ready for production" (which does _not_ mean the easyconfigs are final, or will remain unchanged in the future).


### EasyBuild features supporting this policy

Some of the processes supporting this policy are already automated: running the unit tests on proposed changes is done by Jenkins which flags a PR as red/green, uploading a test report is child's play from the eb command line, etc.

Other parts can/will be automated soon:
 * opening a pull request with new/adjusted easyconfigs (via a command line option like `--new-pr`)
 * reviewing easyconfig PRs w.r.t. consistency with existing easyconfigs (via `--review-pr`, cfr. https://github.com/hpcugent/easybuild-framework/pull/1005)
 * searching for easyconfigs in the current develop branch and open PRs via `eb --search` (which is the reason to prefer PRs over issues as a means of communication)

Others are likely to remain manual, or at best semi-automatic, i.e. the style aspect of reviewing PRs.