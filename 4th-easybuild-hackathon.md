**This hackathon is being planned, details will be completed as they become available.**

### Basic Information

* **date**: Oct. Tuesday 22nd - Thursday 24th 2013
* **location**: The Cyprus Institute, Nicosia (Cyprus)
* **notes**: agenda set, further details later

### Attendees

confirmed list of attendees:
* Yossi Baruch (Isragrid)
* Xavier Besseron (University of Luxembourg)
* Stelios Erotokritou (The Cyprus Institute)
* Fotis Georgatos (University of Luxembourg, HPC sysadmin and active contributor)
* Kenneth Hoste (HPC-UGent, EasyBuild developer and release manager)
* Thekla Loizou (Cyprus Institute, local organization)
* Dina Mahmoud Ibrahim (Cairo University)
* Dr. Bernd Mohr (Jülich Supercomputing Centre, UNITE)
* Alan O'Cais (Jülich Supercomputing Centre)
* Andreas Panteli (The Cyprus Institute)
* George Tsouloupas (The Cyprus Institute)

## Agenda

Initial agenda, subject to change.

We are thinking about setting up a way to allow people to remotely participate in the hackathon (e.g., regular Skype sessions on progress, etc.).

### Tuesday Oct 22nd 2013 (9am - 5pm)

 * introduction to EasyBuild and recently added features
 * introduction to UNITE, current status and open issues
 * hands-on introductory EasyBuild sessions
 * discussing tasks to tackle during EasyBuild hackathon
 * **[1pm - 2pm] lunch provided**

### Wednesday Oct 23rd 2013 (9am - 5pm)

 * hands-on introductory EasyBuild sessions
 * hacking sessions for enhancing EasyBuild and adding support for (new) software (versions) 
 * **[1pm - 2pm] lunch provided**
 * **[8pm] dinner (location TBA)**

### Thursday Oct 24th 2013 (9am - 5pm)

 * hands-on introductory EasyBuild sessions
 * hacking sessions for enhancing EasyBuild and adding support for (new) software (versions) 
 * **[1pm - 2pm] lunch provided**

## Hackathon projects/tasks

Below a couple of potentially interesting open issues that are listed that can be tackled during the hacking sessions:

 * adding support for building and installing additional software packages
    * check if there's an issue already open w.r.t. the software package you want to look into
    * if not, open an issue in the easybuild-easyconfigs repository (see https://github.com/hpcugent/easybuild-easyconfigs/issues/new)
    * check whether HPC-UGent has (outdated) support available, and ask them for the easyconfigs/easyblocks to port when they do
        * see https://github.com/hpcugent/easybuild/wiki/List-of-supported-software-packages/ede46976d7367a86fe76ae79adba7b8e9fd9f118
    * check whether an automatically generated easyconfig is available as a starting point in the easybuild.experimental repository
        * see https://github.com/fgeorgatos/easybuild.experimental/tree/master/contrib/pkgsrc
 * of particular interest: support for more GPGPU applications (e.g., NAMD, ...), performance tools (UNITE, ...), etc.

 * implementing a custom module naming schemes that matches your site policy
    * see https://github.com/hpcugent/easybuild-framework/pull/687)

## Remote participation

Together with the local organizers, we would like to make it possible for people who are not attending the hackathon physically to participate remotely.

The presentations on EasyBuild and UNITE will be recorded, and we will do what we can to make these recordings available as quickly as possible.
We will also try to stream the talks as they happen, so people can tune in remotely (if you have any suggestions on tools for this, let us know).
The slides of the talks will be available up front on the EasyBuild wiki.

Next to streaming and recording the talks, we also plan to schedule conference calls (Skype and/or Google Hangouts) with remote attendees to answer any questions they may have on the work they're doing with EasyBuild.
The idea is that remote participants can try and tackle open issues on EasyBuild (doesn't matter what, any issue you care about), just like the hackathon attendees, and get help when/if they need it.

To try and make these efforts work, it's important to plan ahead and get an idea of who would like to participate remotely.
The main things we would like to get a view on is the number of remote participants, and at which times they are planning to participate.
We would like to schedule the conf calls such that the time at which they take place works well on both ends of the wire, i.e., taking into account time zone differences.

**If you would like to remotely participate in the 4th EasyBuild hackathon, please let us know before Friday Oct 18th 1pm (CET) by filling in the doodle at http://doodle.com/625h6t2teegqp326taxn2ihg/admin?#cmt1168352803** .
Please clearly indicate you'll be participating remote by adding a `(remote)` tag next to your name. Once we have a clear view on the remote participants, we will work out the details together with them.


## Meeting minutes

 * (none yet)
