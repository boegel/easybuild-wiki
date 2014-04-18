(back to [[Conference calls]])

Notes on the 10th EasyBuild conference call, Thursday Apr. 17th 2014 (3pm - 3.30pm CET)

 * follow link in "Video calls" section on [EasyBuild community Google+ page](https://plus.google.com/communities/103632287931200436158)

#### Attendees

Alphabetical list of attendees (4):

* Petar Forai (GMI, Austria)
* Fotis Georgatos (University of Luxembourg)
* Kenneth Hoste (HPC-UGent, Belgium)
* Ward Poelmans (UGent, Belgium)

#### Agenda

* better organize reviewing and testing of (easyconfigs) pull requests (Kenneth)

#### Notes

* more frequent releases of easyconfigs?
    * is of little value, EasyBuild doesn't care where easyconfigs are located (they don't need to be part of a release)
* separate pool of new easyconfigs that are not part of a release yet (or maybe not even merged yet in `develop`)
    * in an 'experimental' branch
    * make `-—search` aware of it
* make handling of pull requests easier for the release
    * list of lieutenants: Petar, Ward, Pablo, Fotis, Jens
    * requires some scripting
        * code style => make it part of unit test suite
        * test report, collects metadata, push result in PR (hub wrapper helps?)
    * guidelines for:
        * opening a PR: build works, # files, etc.
        * reviewing a PR
    * automate testing of the builds, via Jenkins
        * attention point during next hackathon?
* recursive try would be tremendously valuable
    * but will also result in lots of new easyconfigs...
    * easyconfig format v2 will help a lot, but not fully finished yet
