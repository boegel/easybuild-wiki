Notes on the 1st EasyBuild conference call, Tuesday Nov. 5th 2013 (3pm -3.30pm)

 * [planned event](https://plus.google.com/u/0/events/cfarbd6ms0v1d03942uf75h672g?authkey=CJr7g7DJhY6ulwE)
 * [link to Google Hangout](none)

#### Attendees

Alphabetical list of attendees:

* Yossi Baruch (Isragrid)
* Fotis Georgatos (Uni.lu)
* Andy Georges (UGent)
* Kenneth Hoste (UGent)
* Bernd Mohr (JSC)
* Alan O'Cais (JSC)
* George Tsouloupas (CyI)

#### Agenda

Planned topics to be discussed:

* (short) progress on support for new easyconfig format (Kenneth)
* progress report on adding support for UNITE and the individual performance tools that are part of it (Bernd)
* current status of 'toolchain soup' (Ward)
* remaining time is for open questions on broad topics

#### Notes

Notes on topics discussed during the conference call (by Kenneth)

##### Progress report on support for new easyconfig format (Kenneth)

 * pull request for adding initial support for new easyconfig format open, but still WIP (see [framework#693](https://github.com/hpcugent/easybuild-framework/pull/693), [stdweird-framework#22](https://github.com/stdweird/easybuild-framework/pull/22))
  * goal is to merge this in for EB v1.9.0
  * will not provide support for usable easyconfig files in this format yet (most likely)
 * [(very) preliminary documentation available](https://github.com/hpcugent/easybuild/wiki/Easyconfig-format-two)

##### Progress report on adding support for building/installing UNITE (Bernd)

 * 

##### Current status of 'toolchain soup'

 * Q (Ward): _You want to build something with a certain compiler but first you need to update several deps to the correct toolchain version, create new easyconfigs because they only exists for gcc and not ictce (or vice versa), ... Is this something the v2.0 format will solve?_
 * `--try-toolchain --robot` doesn't work as expected yet (see [framework#474](https://github.com/hpcugent/easybuild-framework/issues/474))
  * so, currently requires 'manual' work 
   * module for new toolchain and toolchain elements need to be provided first
   * exhaustive listing of all easyconfig files that need to be rebuilt using other toolchain
 * new easyconfig format should make this a lot less painful, but is no magic solution
  * if easyconfig for `foo` does not have support for building with toolchain `bar`, is will still need to be added somehow (either manually or via `--try-toolchain`)
 * wiki page for pros/cons of minimal vs full toolchains: https://github.com/hpcugent/easybuild/wiki/Minimal-vs-full-toolchains

##### Other topics

###### some_topic

 * 

