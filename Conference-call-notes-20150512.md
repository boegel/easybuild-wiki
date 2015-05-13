(back to [[Conference calls]])

Notes on the 29th EasyBuild conference call, Tuesday May 12th 2015 (3.00pm - 3.30pm CET)

#### Attendees

Alphabetical list of attendees (6):

* Pablo Escobar (UniBas - SIB, Switzerland)
* Petar Forai (IMP/IMBA, Austria)
* Fotis Georgatos (freelancer)
* Eric Gregory (JSC, Germany)
* Kenneth Hoste (HPC-UGent, Belgium)
* Alan O'Cais (JSC, Germany)

#### Agenda

* an overview of the EasyBuild activities at the NeIC conference last week
* an outlook to EasyBuild v2.1.1 (bugfix release which should go out soon, mostly related to --module-only)
* a discussion on how to get more people involved in reviewing/testing pull requests and shorten the turnaround time for incoming contributions


#### Notes

##### EasyBuild activities @ Espoo (Finland) & NeIC conference

* 2-day EasyBuild hackathon @ CSC.fi (see https://github.com/hpcugent/easybuild/wiki/9th-EasyBuild-hackathon)
  * 6-7 attendees, almost all CSC.fi employees (+ one visitor from uit.no on day 2)
  * good response, everyone got their hands dirty
  * most of the work was done on Taito (x86_64 Linux cluster, with Lmod & hierarchical modules)
  * not so much on their Cray system (Sisu)
  * nice result: 20-30% faster CP2K build compared to manual build by local application expert
* invited talk on EasyBuild (and Lmod) at NeIC User Support Workshop
  * http://neic2015.nordforsk.org, https://www.nsc.liu.se/~torbenr/neic2015/
  * followed by a 2-3hr hands-on session
  * good response overall, some people were even amazed
  * contact made with new potential contributors, a couple of newcomers in EasyBuild IRC channel
  * uit.no is planning to push EasyBuild on a national level in Norway, other Nordic countries may follow their lead

##### EasyBuild 2.1.1

* bugfix release, mostly focused on (serious) issues with `--module-only`
  * `module load` statements are missing in generated modules if `--module-only` is combined with `--force` (which it usually is)
  * a large portion of easyblocks were incompatible with `--module-only` (unset variables because of skipping of tests)
   * unit tests were enhanced to make sure easyblocks remain compatible with `--module-only`
* release is imminent, just a couple of other minor things that will be included as well (e.g. support for installing GROMACS v5.x)

##### Lowering turnaround time for (easyconfigs) pull requests

* over 300 open easyconfigs pull requests, lowering that turns out to be difficult due to limited amounts of time
* more automation is required
  * automatic style review (first-pass)
  * better feedback by bot rather than just passing a link
  * automatic 'regtesting' of PR once unit tests and style review pass
  * human factor can't be ruled out entirely, but can be minimised significantly
* Fotis: good exercise is to imagine what would be needed if number of pull requests multiply by factor of 10-100
* more community involvement in reviewing/testing of PRs is required
* can http://gerrithub.io help?
  * alternative interface to GitHub
  * allows voting of pull requests, requiring (multiple) sign-offs, etc.
* merge sprint day with common contributors
  * block a couple of hours in your agenda to help out getting PRs merged in
  * Kenneth will send out a doodle to figure out a suitable date

##### Other topics

* outreach
  * Mozilla Science Lab director seemed interested after mail to Software Carpentry 'discuss' mailing list
  * contact PRACE trainers (suggestion by Fotis, maybe via Alan?)
    * Alan is already kind of doing that
  * possible involvement in PRACE project via Andreas @ Cyprus Institute
    * follow-up of UEABS PRACE benchmark suite project?
      * see http://www.prace-ri.eu/ueabs/
    * Alan is also trying to get a (non-PRACE) project approved that involves EasyBuild for setting up training infrastructure
  * let EasyBuild be a supporting tool in a particular work package of a large PRACE project should be feasible
  * score a talk at HPC-SIG (UK consortium of HPC sites, http://hpc-sig.org/) via Fotis (who is freelancing in UK right now) or Ola (Univ. of Warwick)?

* EasyBuild event @ JSC? (Alan)
  * end June/early July timeframe?
  * to kickstart use of EasyBuild on new JURECA systems
  * to motivate getting (customised) easyblocks/easyconfigs that now only exist 'in-house' @ JSC contributed back upstream
  * discuss interest in using EasyBuild on desktop systems as well
  * preliminary talk about how EasyBuild might work on BlueGene systems