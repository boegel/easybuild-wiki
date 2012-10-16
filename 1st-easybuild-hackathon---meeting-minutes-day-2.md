(Friday Aug. 17th 2012, 10am-6.30pm)

During the second day of the [[1st EasyBuild hackathon]], we finally got to implemented some of the things discussed, and worked together to finish work that was started (open pull requests, ...). These notes were taken by Kenneth Hoste.

## Action points

 * agree on encoding scheme for class names **(see issue [#86](https://github.com/hpcugent/easybuild/pull/86))**
  * prefix with `eb_` (is this PEP008 compliant? should it be `EB_`?)
   * solves issues with software names starting with numbers, keep origin capitilization after prefix
  * encode characters that can't be parted of class name + underscores
   * `-` -> `_minus_`
   * `#` -> `_hash_`
   * `+` -> `_plus_`
   * `_` -> `_underscore_`
  * _(FG)_ implemented this change during the hackathon, and issued a pull request; he will also document the encoding scheme
 * _(KH)_ work together with _(FG)_ to get acquinted with workflow for contributing back
 * _(KH)_ try and get all of _(TW)_'s pull requests merged
 * _(KH)_ update README file with contact info, fix links so they look pretty **(see issue [#94](https://github.com/hpcugent/easybuild/pull/94))**
 * _(KH)_ fix outdated EasyBuild GitHub webpage (was still mentioning easybuild.sh, for example)
 * _(TW)_ finish work on open pull requests
  * processing remarks, finishing work that was started as much as possible
 * _(JT)_ go through pull request for DOLFIN **(see issue [#80](https://github.com/hpcugent/easybuild/pull/80))**
 * _(JT)_ look into required change to hoist all easyblocks in a single easybuild.easyblocks namespace **(see issue [#87](https://github.com/hpcugent/easybuild/pull/87))**
  * don't use single letters anymore in namespace, but do keep easyblocks organized in letter directories