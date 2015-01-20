**Note: this is a proposal, [feedback or suggestions](http://easybuild.readthedocs.org/en/latest/#getting-help) for improvement are much appreciated!**

This is a policy to follow when issuing a pull request (PR) to the [easybuild-easyconfigs repository](https://github.com/hpcugent/easybuild-easyconfigs/pulls). It is intended to better structure the large amount of pull requests for easyconfig files, and help avoid duplicate work among contributors.

* use a standard format for your PR title: ``[<toolchain>] <software>, ... (<tag>)``
* open a PR (with dummy easyconfigs) as soon as you start working on something, tag it with `(WIP)`
* update the PR when it's ready for review/testing by others, tag it with `(TEST)`
* indicate that your PR should be ready to be merged (after review/testing) by using the `(OK)` tag
* easyconfigs should only be considered production ready when the PR is merged into `develop`

More details in the sections below.

### Use a standard format for your PR title

Using a title for your PRs according to the following standard format makes it easy to recognise what it is providing:

``[<toolchain_name>/<toolchain_version>] <software_name> <software_version>, ... (<tag>)``

* since easyconfigs are specific to a particular _toolchain_, clearly indicating the toolchain being used helps to structure the overview of easyconfigs; both the toolchain name and version should be mentioned, enclosing by square brackets;
  * examples: `[intel/2015a]`, `[goolf/1.5.14-no-OFED]`
* mentioning the main software packages for which easyconfigs are provided, together with their respective version(s) speaks for itself
  * examples: `Boost 1.55.0`; `Python 2.7.9, OpenSSL 1.0.1k`;
* tagging your pull request with a short/clear label is useful for indicating its current status; suggested tags are:
  * `(WIP)` to indicate that the PR is still work-in-progress
  * `(TEST)` to invite others to review and test your PR
  * `(OK)` to indicate that the PR is ready to be merged in

Complete examples:
 * `[intel/2015a] Boost 1.55.0 (WIP)`
 * `[goolf/1.5.14-no-OFED] Python 2.7.9, OpenSSL 1.0.1k (OK)`


### Open a PR as soon as you start working on something

As soon as you start working on a (set of) easyconfig file(s), open a **pull request**, and mark it as being a _work in progress_:

* include placeholders for the easyconfigs you're planning to provide, i.e. empty files, partial files (e.g., only containing definitions for `name`, `version`, `toolchain`), or copies of existing easyconfig files for which only the filename has been adjusted
* clearly mention the tag ``(WIP)`` in the title of the pull request

The key point here is to open a pull request **early** rather than late; that way other people who are planning on looking into the same easyconfigs you are working on can reap the benefits of your labor, contact you to help out, or simply test what you consider ready for consumption by others.

Easyconfigs for multiple software packages can be added in a single PR, but keep it reasonable (5 different software packages max., 10 easyconfig files max.).

For software packages that are part of the Common User Experience agreements (right now: Python, R, Perl, next to the intel/foss toolchains), you tag the other VSC members involved using their GitHub account in the description.

Tag other EasyBuilders that you know are may be interested in your work, using their GitHub account name, for example using `@boegel`.


### Update the PR when it's ready for review/testing by others

Once your easyconfigs are ready for testing/review, you upload them in the pull request by pushing to the corresponding branch in your GitHub easybuild-easyconfigs repository.

Submit a test report using the `--upload-test-report` eb command line option (see https://github.com/hpcugent/easybuild/wiki/Review-process-for-contributions#automated-testing-of-easyconfigs-pull-requests), as a confirmation that the easyconfigs are working for you.

Work together with the reviewer/tester to get the PR ready for merging, which is done by the EasyBuild release manager (i.e. me, for now and the foreseeable future).

Some of these steps are already automated: running the unit tests on your proposed changes is done by Jenkins and flagged as red/green in the pull request, uploading a test report is child's play from the eb command line.


### Indicate that your pull request should be ready to be merged in

Change the `(WIP)` in the pull request title to `(OK)`, and preferably add a comment as well (so that followers are notified via e-mail), mentioning something like "ready for review/testing".

Tagging my GitHub account (@boegel) wins you extra bonus points.


### Aftermath

Once the PR is merged into develop, it can be consider "ready for consumption" (which doesn't mean it's final, unchanged, see for example https://github.com/hpcugent/easybuild-easyconfigs/issues/1304).


### EasyBuild features supporting this policy

Other parts can/will be automated soon:
 * opening a pull request with new/adjusted easyconfigs (via an option like --new-pr)
 * reviewing easyconfig PRs w.r.t. consistency with existing easyconfigs (via --review-pr, cfr. https://github.com/hpcugent/easybuild-framework/pull/1005)
 * searching for easyconfigs in the current develop branch and open PRs via eb --search (which is the reason to prefer PRs over issues as a means of communication)

Others are likely to remain manual, or at best semi-automatic, i.e. the style aspect of reviewing PRs.

Feedback is welcome/highly appreciated.