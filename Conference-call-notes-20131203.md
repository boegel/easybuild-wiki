Notes on the 3rd EasyBuild conference call, Tuesday Dec. 3rd 2013 (3pm -3.30pm CET)

 * follow link in "Video calls" section on [EasyBuild community Google+ page](https://plus.google.com/communities/103632287931200436158)

#### Attendees

Alphabetical list of attendees (X):

* Kenneth Hoste (HPC-UGent)

#### Agenda

* update on EasyBuild-related activities at the SC13 conference
* rough outline for development activity for the coming weeks

#### Notes

##### EasyBuild @ SC13

* BoF was very successful [slides](https://github.com/hpcugent/easybuild/wiki/SC13-BoF-session)
 * ~75 attendees
 * first half: lightning talks on Lmod, `HashDist` & `EasyBuild` + questions
 * second half: show-of-hands via Socrative ([results](http://hpcugent.github.io/easybuild/files/SC13_BoF_show-of-hands-results.pdf))
* possible future SPEC certification (Andy)
* pitched as a benchmarking framework, interest from people (Andy knows) at Google, UT Austin, Microsoft, DARPA
* interest by Intel w.r.t. `Intel Cluster Ready` certification
 * side-question by Intel: payed support for EasyBuild?
* users we didn't know about:
 * Idaho National Lab (since March'13)
 * some site in South-Africa
* (very!) similar projects (some we didn't know about):
 * Univ. of Western Australia (Chris Bording); iVEC Build System (`IBS`), all bash, supports ~100 packages
 * LLNL (Todd Gamblin): `Spack`, Python, very similar w.r.t. easyblocks, supports ~10 packages, several cool features (very cool/flexible/powerful cmdline, no strict versions specifications, ...)
 * both are interested to work on merging their project with EasyBuild into 'something new' (long-term!)
 * access to other (big) systems to test EasyBuild on: BlueGene/Cray systems at LLNL (via Todd Gamblin), Cray system in Australia (via Chris Bording)
* talked to PGI about obtaining a free license to test PGI support in EasyBuild
 * seems like that would be possible (follow-up required)

##### Planned EasyBuild development activities

* EasyBuild v1.10 release planned for Wed Dec 18th 2013, see also [[Release schedule]]
 * most likely no shocking new features, mostly support for new software
* in the next couple of weeks:
 * **strong focus on support for easyconfig format 2.0**
 * preparation for a possible EasyBuild tutorial at [HPCKP conference in Barcelona](http://hpckp.org/index.php/anual-meeting/hpckp14) Jan 13-14th 2014  (Jens)
 * preparation for EasyBuild hackathon / training event at Jülich Supercomputer Centre (JSC) in Jan/Feb'14 (Kenneth, Stijn?)
 * [HPC devroom at FOSDEM](hpcugent.github.io/easybuild/fosdem14.html) (Feb 2nd-3rd 2014)
 * completing fully featured support for custom module naming schemes
 * next: focus on support for site customizations
