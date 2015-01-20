(back to [[Conference calls]])

Notes on the 24th EasyBuild conference call, Tuesday January 20th 2015 (3.00pm - 3.30pm CET)

#### Attendees

Alphabetical list of attendees (5):

* Pablo Escobar (UniBas - SIB, Switzerland)
* Petar Forai (IMP/IMBA, Austria)
* Eric Gregory (JSC, Germany)
* Kenneth Hoste (HPC-UGent, Belgium)
* Ward Poelmans (UGent, Belgium)


#### Agenda

   * outlook to EasyBuild v2.0 (Kenneth)
   * [proposal policy for easyconfig pull requests](https://github.com/hpcugent/easybuild/wiki/Policy-for-easyconfig-pull-requests) (Kenneth)


#### Notes

##### Outlook to EasyBuild v2.0

* required:
  * remove/clean up code that supports deprecated behaviour
  * command line option to auto-fix easyconfigs that are broken due to relying on deprecated behaviour
    * Eric: mainly the missing `easyblock = 'ConfigureMake'` line

##### Proposal policy for easyconfig pull requests

* see https://github.com/hpcugent/easybuild/wiki/Policy-for-easyconfig-pull-requests
* conf call attendees agree that it looks sane, and agree it's worth a try
* Fotis is already picking it up for this open PRs
* proposed policy can be applied to existing PRs whenever they are touched, and should be taken into account for new PRs
* additional idea by Robert Schmidt via IRC (post conf call): also include a `{category}` tag

##### Other topics

* SBGrid (https://sbgrid.org)
  * support for ready-to-use 290 'packaged' bio-informatics software packages, for a small(ish) fee
  * contact them, may/should be interested in EasyBuild?

* EasyBuild hackathon in Basel
  * basically overbooked
  * registered attendance by most major Swiss sites, including CSCS (home of Piz Daint)