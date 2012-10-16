When you specify a list of dependencies in your [[Easyconfig files|Easyconfig-files]], EasyBuild will verify that the modules containing each of these dependencies are present before continuing the installation. If the specification-file is part of a set of packages installed at the same time, an installation order is determined that makes sure all dependencies are available at time of installation.

Additionally, dependencies not present on the system or in the current installation set can be automatically installed by providing a `robot`-path (-r or --robot) to `eb`. EasyBuild will then look for a specification-file in this path and add it to set of packages currently being installed.

To be found, the specification-file should be located in [robot-path]/[package-name]/[version-with-options].eb (Hint: this is the same structure used for specification-files sent to source control).

A specification-file can also contain dependencies on certain packages (see the [requirements option](Easyconfig-files#Dependencies), these dependencies however cannot be automatically resolved.
