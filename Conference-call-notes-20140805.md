(back to [[Conference calls]])

Notes on the 15th EasyBuild conference call, Wednesday August 5th 2014 (3.15pm - 3.45pm CET)

 * follow link in "Video calls" section on [EasyBuild community Google+ page](https://plus.google.com/communities/103632287931200436158)

#### Attendees

Alphabetical list of attendees (5):

* Xavier Besseron (University of Luxembourg)
* Petar Forai (GMI, Austria)
* Kenneth Hoste (HPC-UGent, Belgium)
* Ward Poelmans (UGent, Belgium)
* Olav Smørholm (University of Warwick, UK)

#### Agenda

* EasyBuild v1.15 release planning (Kenneth)
* improvements w.r.t. using a dummy/system toolchain (Petar)
* status update of support for hierarchical modules (Kenneth + anyone who has played around with it)

#### Notes

##### Release planning

 * EasyBuild v1.15.0: early Sept'14
 * see [Release schedule](Release-schedule)

##### Lua module file support

 * support for Lua module files in the works by Petar
  * no notion of `conflict` in Lua module files?
  * ask Robert for Lua module file template

##### System compiler (a.k.a. `dummy`)

 * 'dummy' doesn't really make sense, needs to be better documented
 * better name name would be `system`, should be equivalent to `dummy` with an empty version (so dependencies are being loaded)
 * some easyblocks will suffer from this, since they rely on e.g. `self.toolchain.comp_family()`, `mpi_family()`, etc.
 * another (better?) alternative is let EasyBuild install modules that simply wrap system tools
   * we need a `System` (generic) easyblock that can be used to generate modules for system tools (e.g. GCC, OpenMPI, BLAS/LAPACK libraries, etc.) => Xavier?
      *  make `eb` generate a module file, that can then be used as (part of) a toolchain
      * should simply allow for specifying installation paths (for `$PATH`, `$CPATH`, `$LD_LIBRARY_PATH`, etc.)
      * should maybe also support verifying whether the specified version is correct (e.g. check `gcc -V` vs specified version)?

##### Support for hierarchical module naming schemes: status

 * API for custom module naming schemes was enhanced in EasyBuild v1.14.0
    * module name is split up into subdirectory in `$MODULEPATH` (e.g. `Core`) and 'short' module name (e.g. `GCC/4.8.3`)
    * include notion of module path extensions
    * support for inspecting the toolchain, e.g. which compilers, MPI are included in it
    * toolchains can be hidden from users by requesting to not include `module load <toolchain>` statements in generated module files
       * instead, `module load` statements are included for the different toolchain elements
       * this makes a lot of sense in the context of a hierarchy, to avoid useless module files polluting the tree
  * `Apps` vs `Core` for application modules; how to avoid dependency conflicts for 'core' application modules?
  * issue a request for feedback on EasyBuild mailing list on support for hierarchical modules?
