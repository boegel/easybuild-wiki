(Friday Nov. 30th 2012, 9am-4pm)

During the second day of the [[2nd EasyBuild hackathon]], the actual hackathon activities took place. Local attendees started working with EasyBuild, focusing on their interests.

## Hackathon

 * Kenneth:
  * start going through issues in easybuild-framework
   * update milestones, move issues to easybuild-easyblocks or easybuild-easyconfigs if appropriate, update if needed, ...
  * help out people set up enviroment to work with their own easyblocks workspace (this should really be documented well, or scripted if possible)
   * both `easybuild` and `easybuild/easyblocks` directory should have their own `__init__.py` file
   * users' easyblocks workspace should be added to *the end* of the `PYTHONPATH`, to avoid breaking EasyBuild (?)
  * give brief demo on contributing back to EasyBuild (pull request, review cycle, ...)

* Xavier:
  * look into building svn with EasyBuild
  * OpenMPI with no-OFED suffix doesn't make sense, because OpenMPI's configure auto-detects OFED libs
  * will look into fixing HDF5 easyblock such that CMake is used for configuring builds of recent HDF5 versions

 * Valentin:
  * bash easyconfig
  * Vesta (?) (visualisation tool) support

 * Josh
  * Espresso easyconfig (configure/make/make install) + easyblock to automate testing

 * Fotis
  * installing v1.0.1 @ Uni.lu
  * request to extend current list of module classes to include bio (see https://github.com/hpcugent/easybuild-framework/pull/368)

 * Alejandro
  * interest in QuantumESPRESSO, but unable to port/fix old easyblock we have (not familiar with Python)
  * issue opened, see https://github.com/hpcugent/easybuild-easyblocks/issues/49