(back to [[Conference calls]])

Notes on the 20th EasyBuild conference call, Wednesday November 14th 2014 (4.00pm - 4.30pm CET)

#### Attendees

Alphabetical list of attendees (7):

* Timothy Brown (University of Colorado, US)
* Petar Forai (GMI, Austria)
* Kenneth Hoste (HPC-UGent, Belgium)
* Alan O'Cais (JSC, Germany)
* Ward Poelmans (UGent, Belgium)
* Robert Schmidt (OHRI, Canada)
* Olav Smørholm (University of Warwick, UK)

#### Agenda

* revamped EasyBuild documentation at http://easybuild.readthedocs.org (Kenneth)
* outlook to EasyBuild v1.16.0 and v2.0 (Kenneth)
* improving HierarchicalMNS w.r.t. co-compilers (e.g., CUDA) (Olav)

#### Notes

##### revamped EasyBuild documentation

* see http://easybuild.readthedocs.org
* missing topics:
     * make system tools accessible
          * ‘just’ provide a module, maybe .eb too

##### outlook to EasyBuild v1.16.0 and v2.0

* v1.16 release pushed back due to focus on revamped documentation
* still a couple of open framework issues/PRs that should be resolved first
* cfr. https://github.com/hpcugent/easybuild-framework/pulls and https://github.com/hpcugent/easybuild-framework/issues

* deprecation aspect should be handed fully in EasyBuild v1.16.0, to prepare for EasyBuild v2.0 where deprecated behavior will be dropped
     * EasyBuild v2.0 should be the first release of 2015

##### issues with HierarchicalMNS

* Alan/Eric:
     * issue when a tool (GMP) built with another compiler (not in the toolchain) is brought in, since suddenly MPIs are being swapped by Lmod and lots of stuff is rendered inactive
     * move ‘module use’ to toolchains instead?
     * collapse compiler into GMP module to avoid extra extensions to $MODULEPATH causing problems?

* only icc module needs to be loaded to expand $MODULEPATH and make MPI module available
     * access to (also Fortran) application modules can be obtained without ever loading the ifort module...
     * solution?
          * make ‘module use’ in icc/ifort modules conditional on companion module being loaded already?
          * can Lmod spider handle this?
     * cfr. https://github.com/hpcugent/easybuild-framework/issues/1076

* nicely handling co-processors in HIerarchicalMNS
     * e.g. CUDA on top of icc/ifort, see https://github.com/hpcugent/easybuild-framework/pull/1086
     * HierarchicalMNS is not aware of CUDA on top of icc/ifort yet
     * how should co-processors be handled properly in HMNS?

##### Other topics

* Alan/Eric: issue w.r.t. versionsuffix not being honored under HMNS
     * will reproduce and open an issue for this
* next hackathon?
     * Feb 9-11 2015, Basel (Switzerland)
          * Robert McLay (TACC/Lmod) will be joining in too
          * 1st day: introductory topics and site presentations
          * day 2-3: hands-on, with basic (using eb, writing easyconfig files) and advanced (easyblocks/framework features)