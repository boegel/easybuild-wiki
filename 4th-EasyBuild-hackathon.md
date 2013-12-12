### Basic Information

* **date**: Oct. Tuesday 22nd - Thursday 24th 2013
* **location**: The Cyprus Institute, Nicosia (Cyprus)
* **notes**: -

### Attendees

confirmed list of attendees:
* Yossi Baruch (Isragrid)
* Xavier Besseron (University of Luxembourg)
* Marios Constantinou (University of Cyprus)
* Stelios Erotokritou (The Cyprus Institute)
* Fotis Georgatos (University of Luxembourg, HPC sysadmin and active contributor)
* Kenneth Hoste (HPC-UGent, EasyBuild developer and release manager)
* Thekla Loizou (Cyprus Institute, local organization)
* Dina Mahmoud Ibrahim (Cairo University)
* Dr. Bernd Mohr (Jülich Supercomputing Centre, UNITE)
* Alan O'Cais (Jülich Supercomputing Centre)
* Andreas Panteli (The Cyprus Institute)
* Savvas Polydorides (University of Cyprus) **[excused]**
* George Tsouloupas (The Cyprus Institute)

(registered) _remote_ attendees:
* Ward Poelmans (Ghent University, Belgium)
* Adam DeConinck (NVIDIA Corp.)
* Bill Broadley (UC Davis)
* Petar Forai (Gregor Mendel Institute, Austria)

## Agenda

Initial agenda, subject to change.

We are thinking about setting up a way to allow people to remotely participate in the hackathon (e.g., regular Skype sessions on progress, etc.).

### Tuesday Oct 22nd 2013 (9am - 5pm)
 * [9.00am - 9.45am] **welcome, getting started**
  * [9.00am - 9.15am] setup for remote participation
  * [9.15am - 9.30am] Welcome (Jens Wiegand) **([slides](http://hpcugent.github.io/easybuild/files/EasyBuild_hackathon_Cyprus_Oct13_welcome_LinkSCEEM.pdf))**
  * [9.30am - 9.45am] round-table: introduce yourself
 * [9.45am - 11.30am] **presentations** on EasyBuild and UNITE
  * [9.45am - 10.15am] Introduction to UNITE, current status and open issues (Dr. Bernd Mohr) **([slides](http://hpcugent.github.io/easybuild/files/EasyBuild_hackathon_Cyprus_Oct13_UNITE.pdf))**
  * [10.15am - 11.00am] Introduction to EasyBuild (Kenneth Hoste) **([slides](http://hpcugent.github.io/easybuild/files/EasyBuild_introduction_hackathon-Cyprus-Oct13.pdf))** **(recorded presentation: [part 1](http://www.youtube.com/watch?v=bOeNsfLB2t4) - [part 2](http://www.youtube.com/watch?v=e7fyHtO8_qs))**
  * [11.00am - 11.30am] EasyBuild status update (Kenneth Hoste) **([slides](http://hpcugent.github.io/easybuild/files/EasyBuild_status-update_hackathon-Cyprus-Oct13.pdf))** **([recorded presentation](http://www.youtube.com/watch?v=A140WvbqaNw))**
 * [11.45am - 1pm] explaining contribution workflow with `git`, tasks for EasyBuild hackathon
 * **[1pm - 2pm] lunch provided**
 * [2pm - 5pm] hands-on introductory EasyBuild sessions, getting the hackathon started

**Note: all presentations were recorded by Alan, material will be made available when it has been processed.**

### Wednesday Oct 23rd 2013 (9am - 5pm)

 * hands-on introductory EasyBuild sessions
 * hacking sessions for enhancing EasyBuild and adding support for (new) software (versions) 
 * **[1pm - 2pm] lunch provided**
 * **[8pm] dinner (location TBA)**

### Thursday Oct 24th 2013 (9am - 5pm)

 * hands-on introductory EasyBuild sessions
 * hacking sessions for enhancing EasyBuild and adding support for (new) software (versions) 
 * **[1pm - 2pm] lunch provided**
 * [2pm - 5pm] contributing back the work done during the hackathon: opening pull requests
 * [4pm - 5pm] hackathon round-up: what got done, what's left to do

## Hackathon projects/tasks

Below a couple of potentially interesting open issues are listed that can be tackled during the hacking sessions:

#### Get started with EasyBuild

Try and get EasyBuild working on your system(s) of interest:
 * install EasyBuild
 * run the framework unit tests:
```bash
python -m test.framework.suite
```
 * build and install a toolchain (**note: may take a while (hours)**)
 * build and install one or more of the [supported software packages](https://github.com/hpcugent/easybuild/wiki/List-of-supported-software-packages)

#### Review an open pull request, provide feedback

We maintain a "two pairs of eyes" policy for contributions, i.e. someone else than the other has to review a contribution for it can be merged in (see also [Review process](https://github.com/hpcugent/easybuild/wiki/Contributing-back#review-process)).

Pick any open pull request you care about, review it, and provide feedback:

[easybuild-framework](https://github.com/hpcugent/easybuild-framework/pulls) - [easybuild-easyblocks](https://github.com/hpcugent/easybuild-easyblocks/pulls) - [easybuild-easyconfigs](https://github.com/hpcugent/easybuild-easyconfigs/pulls)

#### Implementing a custom module naming scheme that matches your site poilcy

Since version 1.8.0, EasyBuild has support for using **custom module naming schemes**. If your site maintains a strict policy w.r.t. module naming, try and implement it with the support EasyBuild currently has, and supply feedback on **what's missing or what can be improved**.

 * see [merged pull request](https://github.com/hpcugent/easybuild-framework/pull/687) and [wiki page with documentation](https://github.com/hpcugent/easybuild/wiki/Using-a-custom-module-naming-scheme)


#### Adding support for building and installing additional software packages
 
 * check if there's an issue already open w.r.t. the software package you want to look into
  * if not, [open an issue in the easybuild-easyconfigs repository](https://github.com/hpcugent/easybuild-easyconfigs/issues/new)
  * check whether HPC-UGent has (outdated) support available, and ask them for the easyconfigs/easyblocks to port when they do
  ** see [legacy list of supported software](https://github.com/hpcugent/easybuild/wiki/List-of-supported-software-packages/ede46976d7367a86fe76ae79adba7b8e9fd9f118)
  * check whether an automatically generated easyconfig is available as a starting point in the [easybuild.experimental repository](https://github.com/fgeorgatos/easybuild.experimental/tree/master/contrib/pkgsrc)
 * of particular interest: support for more **GPGPU applications** (e.g., NAMD, ...), **performance tools** (UNITE, ...), etc.

#### Implementing requested features

Try and implement a feature that was requested, for example:

 * **a better 'quick demo for the impatient'** ([framework/#442](https://github.com/hpcugent/easybuild-framework/issues/442))
 ** something that's actually _quick_, and can be used for tutorials, workshops, introductions
 ** compiler toolchain based on `TinyCC`, with a default (slow) netlib `BLAS` and `LAPACK` (without MPI support)?
 * fail early when sanity check paths/commands are not what they should be ([framework/#703](https://github.com/hpcugent/easybuild-framework/issues/703))
 * define toolchain environment variables in toolchain modules ([framework/#604](https://github.com/hpcugent/easybuild-framework/issues/604))
 * make sure that all easyconfig parameters are available for alternative module naming schemes ([framework#687](https://github.com/hpcugent/easybuild-framework/issues/687))
 * trip over unknown configure options ([easyblocks/#157](https://github.com/hpcugent/easybuild-easyblocks/issues/157))
 * cmake: add support for "out-of-source" build ([easyblocks/#215](https://github.com/hpcugent/easybuild-easyblocks/issues/215))

Browse through the lists of open issues, and pick a feature request you care about:

[easybuild-framework](https://github.com/hpcugent/easybuild-framework/issues?state=open) - [easybuild-easyblocks](https://github.com/hpcugent/easybuild-easyblocks/issues?state=open) - [easybuild-easyconfigs](https://github.com/hpcugent/easybuild-easyconfigs/issues?state=open)


## Remote participation

Together with the local organizers, we would like to make it possible for people who are not attending the hackathon physically to participate remotely.

 * the **presentations** on EasyBuild and UNITE **will be recorded**, and we will do what we can to make these recordings available as quickly as possible.
 * we will also try to **stream the talks** as they happen, so people can tune in remotely (if you have any suggestions on tools for this, let us know).
 * the **slides** of the talks will be available up front on the EasyBuild wiki.
 * we plan to schedule **conference calls** (Skype and/or Google Hangouts) with remote attendees to answer any questions they may have on the work they're doing with EasyBuild _(see schedule below)_
 * the **[`#easybuild` IRC channel](https://github.com/hpcugent/easybuild/wiki/Contact#irc)** is the prime way for communicating with (remote) attendees and other EasyBuild enthusiasts outside of the hackathon conference calls

The idea is that remote participants can try and tackle open issues on EasyBuild (doesn't matter what, any issue you care about), just like the hackathon attendees, and get help when/if they need it.

#### Preparation

To try and make these efforts work, it's important to **plan ahead** and get an idea of who would like to participate remotely.

The main things we would like to get a view on is:
 * the number of remote participants
 * at which times they are planning to participate

We would like to schedule the conference calls such that the time at which they take place works well on both ends of the wire, i.e., taking into account **time zone differences**.

#### Register your remote participation

_**If you would like to remotely participate in the 4th EasyBuild hackathon, please let us know before Friday Oct 18th 2013 1pm (CET) by filling in the doodle at http://doodle.com/625h6t2teegqp326.**_

Please clearly indicate you'll be participating remote by adding a `(remote)` tag next to your name. Once we have a clear view on the remote participants, we will work out the details together with them.

### Conference calls

Conference calls will be done using **Google Hangouts** (via Google Plus), since that's the least restrictive (access to Google Plus is free, group conference calls on Skype now require the non-free Skype Premium option).

#### Schedule

Cypriot time | potential remote attendees | notes
:--: | :-- | :--
8.15am | Bill & Adam (10.15pm)  | before hackathon starts _(possible?)_
1.30pm | Ward & Petar (12.30pm)  | during hackathon lunch break
6pm | Ward & Petar (5pm), Bill & Adam (8am) | after hackathon
11pm (?) | Ward & Petar (10pm), Bill & Adam (1pm) | after local dinner _(exact time uncertain)_

## Notes

 * [EasyBuild cheat sheet](http://hpcugent.github.io/easybuild/files/EasyBuild_cheatsheet_Oct13.pdf)
 * [Tuesday: presentations, discussions and first hands-on experiences](https://github.com/hpcugent/easybuild/wiki/4th-easybuild-hackathon---meeting-minutes-day-1)
 * [Wednesday: hackathon, discussions](https://github.com/hpcugent/easybuild/wiki/4th-easybuild-hackathon---meeting-minutes-day-2)
 * [Thursday: hackathon (continued), discussions](https://github.com/hpcugent/easybuild/wiki/4th-easybuild-hackathon---meeting-minutes-day-3)
