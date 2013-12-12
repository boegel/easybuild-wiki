(back to [[Conference calls]])

Notes on the 1st EasyBuild conference call, Tuesday Nov. 5th 2013 (3pm -3.30pm)

 * [planned event](https://plus.google.com/u/0/events/cfarbd6ms0v1d03942uf75h672g?authkey=CJr7g7DJhY6ulwE)
 * link to Google Hangout: https://plus.google.com/116729781669672561222/posts/3LfHWAy7dDy
  * note: direct link breaks easily, should follow link in "Video calls" section on [EasyBuild community Google+ page](https://plus.google.com/communities/103632287931200436158) instead

#### Attendees

Alphabetical list of attendees (11):

* Yossi Baruch (Isragrid)
* Xavier Besseron (Uni.lu)
* Fotis Georgatos (Uni.lu)
* Andy Georges (HPC-UGent)
* Kenneth Hoste (HPC-UGent)
* Bernd Mohr (JSC)
* Alan O'Cais (JSC)
* Andreas Panteli (CyI)
* Ward Poelmans (UGent)
* Jens Timmerman (HPC-UGent)
* George Tsouloupas (CyI)

#### Agenda

Planned topics to be discussed:

* (short) progress on support for new easyconfig format (Kenneth)
* progress report on adding support for UNITE and the individual performance tools that are part of it (Bernd)
* current status of 'toolchain soup' (Ward)
* remaining time is for open questions on broad topics, which included:
 * loading various profiler/debugger modules at the same time, using them at the same time (Fotis)

#### Notes

Notes on topics discussed during the conference call (by Kenneth)

Actual timing: 3.10pm - 3.45pm (CET)

##### General notes on conference call setup

 * hard limit of 10 attendees in Google Hangout
  * side-note: people at same site should sit together if feasible
 * no easy way to obtain a reliable direct link that can be communicated shortly before the conf call to let people join
  * easiest way to join in is to follow the "Video calls" link on the [EasyBuild Google+ community page](https://plus.google.com/communities/103632287931200436158)

##### Progress report on support for new easyconfig format (Kenneth)

 * pull request for adding initial support for new easyconfig format open, but still WIP (see [framework#693](https://github.com/hpcugent/easybuild-framework/pull/693), [stdweird-framework#22](https://github.com/stdweird/easybuild-framework/pull/22))
  * goal is to merge this in for EB v1.9.0
  * will not provide support for usable easyconfig files in this format yet (most likely)
 * [(very) preliminary documentation available](https://github.com/hpcugent/easybuild/wiki/Easyconfig-format-two)
 * (Bernd) will section markers for dependencies (e.g. `[dep:PAPI > 4]`) also be supported?
  * (Kenneth) in the end, yes, but not initially
 * (Ward) how will patch file specifications be handled?
  * (Kenneth) a default list of patch files can be specified, version or toolchain specific patches can be added in corresponding sections
 * (George) what about conditional constructs that are currently used in easyconfig files?
  * (Kenneth) these will still be possible in the Python header part, but typically indicate a missing feature that should be implemented in the framework (or the need for an easyblock)
 * (Andy) will section markers be hierarchical?
  * (Kenneth) yes, e.g. a `goolf` toolchain section with a recent version section below that:
```
[goolf]
foo=bar
[[> 2.0]]
foo=rab
```

##### Progress report on adding support for building/installing UNITE (Bernd)

 * UNITE is a set of performance tools packaged together, in a framework that makes it easy to install and use them (see http://apps.fz-juelich.de/unite/)
 * easyconfig files for about 24 of these performance tools were produced during and after the last hackathon
  * only for the `gompi` toolchain, following the 'minimal toolchain' approach (i.e. some easyconfigs only use `GCC` as toolchain)
  * also needs to be tested for `goolf`, `ictce`, etc.
  * another 10-15 tools can be added to this set
 * easyconfig files will be contributed to the community soon
  * (KH) already done in mean time, see [easybuild.experimental - users/unite](https://github.com/fgeorgatos/easybuild.experimental/blob/master/users/unite) (make sure to check the `README.md` file!)
 * some rather annoying issues on openSuSE turned up w.r.t. `lib64` vs `libexec`, e.g. for `GCC`, `OpenMPI`, `Paraver`
  * see [easyblocks#283](https://github.com/hpcugent/easybuild-easyblocks/issues/283)

##### Current status of 'toolchain soup'

 * question by Ward: _You want to build something with a certain compiler but first you need to update several deps to the correct toolchain version, create new easyconfigs because they only exists for gcc and not ictce (or vice versa), ... Is this something the v2.0 format will solve?_
 * `--try-toolchain --robot` doesn't work as expected yet (see [framework#474](https://github.com/hpcugent/easybuild-framework/issues/474))
  * so, currently requires 'manual' work 
   * module for new toolchain and toolchain elements need to be provided first
   * exhaustive listing of all easyconfig files that need to be rebuilt using other toolchain
 * new easyconfig format should make this a lot less painful, but is no magic solution
  * if easyconfig for `foo` does not have support for building with toolchain `bar`, is will still need to be added somehow (either manually or via `--try-toolchain`)
 * wiki page for pros/cons of minimal vs full toolchains: https://github.com/hpcugent/easybuild/wiki/Minimal-vs-full-toolchains
  * lets try and document pros and cons of both approaches there, and then revisit the discussion once it's worked out
 * `dummy` toolchain is mainly intended for constructing compiler toolchains (e.g. for building `GCC` as the compiler for `goolf`
  * can in principal also be used for 'compiler-independent' software packages (e.g. GDB, Valgrind, CMake), but should be done with great care (e.g. no dynamically linked libraries provided by compiler, because loading an (older) compiler module may break the tool)

##### Other topics

###### Loading (and using) multiple debuggers/profilers together (Fotis)

 * question by Fotis, mainly directed to Bernd: can multiple debuggers or profilers be loaded at the same time?
  * (Bernd) loaded yes, should probably work, but using multiple profilers at the same time on a particular applicaton will not work due to low-level trickery
  * (Xavier) using multiple debugger instances (e.g. two GDBs) on the same running process will not work because of signal handling
  * (Fotis) question was more targeted on just having multiple debugger/profiler modules loaded at the same time
  * (Xavier) little use in that?
  * (Bernd) should work, just like with having both GCC and Intel compilers available on your system
  * (Kenneth) would work, but may yield differnt behavior (e.g. having only `icc` module loaded vs having both `icc` and `GCC` loaded; latter would make `icc` use different libraries provided by GCC module (as opposed to system GCC)

