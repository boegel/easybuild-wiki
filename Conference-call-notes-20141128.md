(back to [[Conference calls]])

Notes on the 21st EasyBuild conference call, Wednesday November 28th 2014 (3.00pm - 3.30pm CET)

#### Attendees

Alphabetical list of attendees (7):

* Petar Forai (GMI, Austria)
* Fotis Georgatos (freelancer)
* Kenneth Hoste (HPC-UGent, Belgium)
* Ward Poelmans (UGent, Belgium)
* Robert McLay (TACC, US)
* Olav Smørholm (University of Warwick, UK)
* Jens Timmerman (HPC-UGent, Belgium)

#### Agenda

* overview of EasyBuild events and discussions at SC14 (Petar, Jens)


#### Notes

#####  overview of EasyBuild events and discussions at SC14

* most people already knew about EasyBuild & Lmod
  * interest by several commercial companies in Lmod
* lots of 'grumpy Jims', small sites drowning in work
* excellent talk by Markus at HUST14
  * quite a bit of discussion afterwards
  * post-talk questions (thanks Robert for keeping track):
    * How to install w/ EasyBuild where you have local disks?
    * How to build the next version when you get a new compiler version?
    * Software vendors should support these tools?
    * Does Lmod support Tcl modules?
    * A comment about Lmod at PNL
    * Is there way for EasyBuild to install Intel?

* alternative tool: Spack
  * has nice features: advanced dependency handling support, powerful command line
  * now also supports generating module files
  * merge between EasyBuild and Spack should be discussed again with Todd Gamblin (LLNL)
    * licensing allows merging Spack (LGPL) into EasyBuild (GPLv2) already
    * one way may be to host both tools in a single repository, and merge gradually
* EasyBuild & Lmod were promoted by Olli-Pekka (CSC, Finland) at (private) Cray meeting

#### Other topics

* next EasyBuild release?
  * EasyBuild v1.16 mid-December 2014
  * delayed due to revamped EasyBuild documentation, SC14, HPC-UGent site maintenance, etc.
  * will support disabling deprecated behaviour, to prepare for EasyBuild v2.0 (1st release of 2015)
  * issues uncovered during 2-day hackathon at JSC in Oct'14 should be mostly fixed

* Lmod plans
  * the open 'matrix' problem
    * multiple parents in a hierarchy (e.g., OpenBLAS vs Intel MKL)
      * gets even more complex with BLAS+FFTW (1 parent with Intel MKL, two parents with OpenBLAS/FFTW)
    * solution @ TACC for now is to use Rpath rather than prereqs
    * basically a graph problem, so talk to someone doing graph research?
       * dynamic path search in a multi-dimensional graph with limited visibility
    * talk to Todd about this, since he has advanced dependency resolution support in Spack
    * take a look at how dependencies are handled in Debian
       * talk to Stefano Zacchiroli?
          * Debian project lead, led the effort behind CUDF, project Mancoosi
          * may be attending FOSDEM
