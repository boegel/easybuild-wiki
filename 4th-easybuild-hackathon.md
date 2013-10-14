**This hackathon is being planned, details will be completed as they become available.**

### Basic Information

* **date**: Oct. Tuesday 22nd - Thursday 24th 2013
* **location**: The Cyprus Institute, Nicosia (Cyprus)
* **notes**: agenda set, further details later

### Attendees

confirmed list of attendees:
* Yossi Baruch (Isragrid)
* Stelios Erotokritou (The Cyprus Institute)
* Fotis Georgatos (University of Luxembourg, HPC sysadmin and active contributor)
* Kenneth Hoste (HPC-UGent, EasyBuild developer and release manager)
* Thekla Loizou (Cyprus Institute, local organization)
* Dina Mahmoud Ibrahim (Cairo University)
* Dr. Bernd Mohr (Jülich Supercomputing Centre, UNITE)
* Alan O'Cais (Jülich Supercomputing Centre)
* Andreas Panteli (The Cyprus Institute)
* George Tsouloupas (The Cyprus Institute)
* ...

## Agenda

Initial agenda, subject to change.

We are thinking about setting up a way to allow people to remotely participate in the hackathon (e.g., regular Skype sessions on progress, etc.).

### Tuesday Oct 22nd 2013 (9am - 5pm)

 * introduction to EasyBuild and recently added features
 * introduction to UNITE, current status and open issues
 * hands-on introductory EasyBuild sessions
 * discussing tasks to tackle during EasyBuild hackathon
 * [1pm - 2pm] lunch provided

### Wednesday Oct 23rd 2013 (9am - 5pm)

 * hands-on introductory EasyBuild sessions
 * hacking sessions for enhancing EasyBuild and adding support for (new) software (versions) 
 * [1pm - 2pm] lunch provided
 * [8pm] dinner (location TBA)

### Thursday Oct 24th 2013 (9am - 5pm)

 * hands-on introductory EasyBuild sessions
 * hacking sessions for enhancing EasyBuild and adding support for (new) software (versions) 
 * [1pm - 2pm] lunch provided

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

## Meeting minutes

 * (none yet)