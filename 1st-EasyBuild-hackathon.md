### Basic Information

* **time**: Thu-Fri Aug 16-17 2012, 10am-5pm CET
* **location**: UGent (Belgium), campus Sterre, building S9, meeting room Aubergine ([more info](http://www.ugent.be/hpc/en/contact))
* **notes**: lunch provided on Thursday by HPC-UGent

### Attendees

5 attendees:
* Stijn Deweirdt (HPC-UGent, developer)
* Fotis Georgatos (University of Luxembourg, contributor/sysadmin)
* Kenneth Hoste (HPC-UGent, developer/release manager)
* Jens Timmerman (HPC-UGent, developer)
* Toon Willems (HPC-UGent summer intern, developer)

## Agenda

### presentations
note: informal presentations, no need to prepare slides
 * _(Fotis)_ development timeline
 * _(Fotis)_ overview of current open bug/feature requests
 * _(Fotis/Cédric)_ experience report by Fotis and Cédric

###  discussions
 * _(Kenneth)_ public bug tracker (GitHub issue tracker?)
 * _(Kenneth)_ policy for [contributing back](Contributing back) to EasyBuild (feature branches, pull requests, code review, release manager)
 * _(Fotis/Cédric)_ [wishlist of Fotis and Cédric](https://github.com/fgeorgatos/easybuild/wiki/Wishlist)
  * namespace format and and recommendations by the EasyBuild team
  * configuring, library building and related potential issues
  * how to handle `osdependencies` gracefully
   * extension: how to describe package info & dependencies (employ [CUFD](http://www.mancoosi.org/cudf/)?) 
 * _(Fotis)_ class naming (`eb_` prefix)
 * _(Fotis)_ can we benefit in any way from pkgsrc/ports/portage etc?

###  hacking
 * bug fixing
 * cleaning up work by Fotis and Cédric
 * finish work for v0.9
 * new features:
  * git-download class for RAxML (see cbench)

## Meeting minutes

 * [meeting minutes day 1: discussions](1st-EasyBuild-hackathon---meeting-minutes-day-1)
 * [meeting minutes day 2: hacking](1st-EasyBuild-hackathon---meeting-minutes-day-2)