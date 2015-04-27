(back to [[Conference calls]])

Notes on the 27th EasyBuild conference call, Tuesday March 17th 2015 (2.00pm - 2.30pm CET)

#### Attendees

Alphabetical list of attendees (5):

* Petar Forai (IMP/IMBA, Austria)
* Eric Gregory (JSC, Germany)
* Kenneth Hoste (HPC-UGent, Belgium)
* Ward Poelmans (UGent, Belgium)
* Robert Schmidt (OHRI, Canada)


#### Agenda

   * outlook to EasyBuild v2.1
   * discussion of ideas for new features
   * open floor for questions/problems

#### Notes

##### EasyBuild v2.1 outlook

   * strict deadline: end of April (before EasyBuild hackathon @ CSC.fi)
   * Cray support should/must be included by then
     * different components:
       * support for listed 'system modules' as dependencies (PR coming up by Petar)
       * definitions of Cray toolchains
       * script to generate easyconfigs for Cray toolchains, so modules can be installed for them
   * other features will be merged in as they are ready (cfr. https://github.com/hpcugent/easybuild/issues/100)
      * support for module files in Lua syntax is very close to being ready (https://github.com/hpcugent/easybuild-framework/pull/1060)
        * opens up possibilities to further enhance synergy with Lmod
        * also supporting properties and families will be looked into later (EB 2.2?)
      * support for `--review-pr` is close to being finished (https://github.com/hpcugent/easybuild-framework/pull/1142)
      * deprecating `log.error` in favour of `raise EasyBuildError` is likely to go in (https://github.com/hpcugent/easybuild-framework/pull/1218)
      * getting support for GC3Pie backend for `--job` ready is being looked into, unclear whether that will be fully stable by EB v2.1 (https://github.com/hpcugent/easybuild-framework/pull/1008)

##### Support for --hide-deps (Pablo)

  * certainly interesting for JSC as well to avoid accidental installations of non-hidden zlib
  * better option than adding support for `hidden = True` in easyconfigs, so that would also require listing that dependency via `hiddendependencies` in other easyconfigs

##### Docker test setup

   * Robert: Docker setup for automated testing PRs
    * focused on `foss` toolchains for now
    * packaging support would help to avoid rebuilding everything from scratch (seed toolchain build via RPMs)
    * also interested to look into pulling in required easyblock PRs
     * just looking for easyblocks PR URL in easyconfig PR description is probably sufficient

##### Other topics

   * interesting future avenue: Nix (http://nixos.org) as a way to set up a build environment for EB
   * messed up permissions, e.g. `blist` is not world-readable (Eric)
     * will open issue to collect more details and look into a fix
     * use `adjust_permissions` in `post_install_step` to make sure everything is world-readable?
   * Robert has been working on adding packaging support (cfr. https://github.com/hpcugent/easybuild-framework/pull/1170)
     * new PR will be opened to proceed with that and have an easy way to discuss/test/give feedback
     * see https://github.com/hpcugent/easybuild-framework/pull/1224