# Build frameworks naming conventions

## macports

|name|what the function is about|
|----|--------------------------|
|fetch|Fetch the ${distfiles} from ${master_sites} and place it in ${prefix}/var/macports/distfiles/${name}|
|checksum|Compare ${checksums} specified in a Portfile to the checksums of the fetched ${distfiles}.|
|extract|Unzip and untar the ${distfiles} into the path ${prefix}/var/macports/build/..../work|
|patch|Apply optional patch files specified in ${patchfiles} to modify a port's source code file(s).|
|configure|Execute the command configure in ${worksrcpath}.|
|build|Execute ${build.cmd} in ${worksrcpath}.|
|test|Execute commands to run test suites bundled with a port.|
|destroot|Execute the command make install DESTDIR=${destroot}in ${worksrcpath}.|

Ref. http://guide.macports.org/#reference.phases -> 5.3. Port Phases

## portage

|name|what the function is about|
|----|--------------------------|
|pkg_setup	 |Use this function to perform any miscellaneous prerequisite tasks. This might include checking for an existing configuration file.|
|pkg_nofetch	 |Inform the user about required actions if for some reason (such as licensing issues) the sources may not be downloaded by Portage automatically. Use this in conjunction with RESTRICT="fetch". You only should display messages in this function, never call die.|
|src_unpack|	 |Use this function to unpack your sources, apply patches, and run auxiliary programs such as the autotools. By default, this function unpacks the packages listed in A. The initial working directory is defined by WORKDIR.|
|src_compile	 |Use this function to configure and build the package. The initial working directory is S.|
|src_install	 |Use this function to install the package to an image in D. If your package uses automake, you can do this simply with emake DESTDIR="${D}" install. Make sure your package installs all its files using D as the root! The initial working directory is S.|
|src_test	 |Executed only when FEATURES="test" is set and RESTRICT="test" is unset, the default of this function executes an available testing function from any Makefiles in the ${S} directory, running either "make test" or "make check" depending on what is provided. It can be overriden to create a custom test setup.|
|pkg_preinst	 |The commands in this function are run just prior to merging a package image into the file system.|
|pkg_postinst	 |The commands in this function are run just following merging a package image into the file system.|
|pkg_prerm	 |The commands in this function are run just prior to unmerging a package image from the file system.|
|pkg_postrm	 |The commands in this function are run just following unmerging a package image from the file system.|
|pkg_config	 |You use this function to set up an initial configuration for the package after it's installed. All paths in this function should be prefixed with ROOT which points to user-specified install root which may not happen to be /. This function is only executed if and when the user runs: emerge --config =${PF}.|

Ref: http://www.gentoo.org/proj/en/devrel/handbook/handbook.xml?part=2&chap=1#doc_chap2 -> Functions

## pkgsrc

name|what the function is about
----|--------------------------
depends| to build and install dependencies
fetch| to fetch distribution file(s)
checksum| to fetch and check distribution file(s)
extract| to look at unmodified source
patch| to look at initial source
configure| to stop after configure stage
all or build| to stop after build stage
stage-install| to install under stage directory
test| to run package's self-tests, if any exist and supported
package| to create binary package before installing it
replace| to change (upgrade, downgrade, or just replace) installed package in-place
deinstall| to deinstall previous package
package-install| to install package and build binary package
install| to install package
bin-install| to attempt to skip building from source and use pre-built binary package
show-depends| print dependencies for building
show-options| print available options from options.mk

Ref: http://wiki.netbsd.org/pkgsrc/targets/


## ports

name|what the function is about
----|--------------------------
config|	Configure OPTIONS for this port using dialog(1).
fetch|	Fetch all of the files needed to build this port from the sites listed in MASTER_SITES and PATCH_SITES.  See FETCH_CMD, MASTER_SITE_OVERRIDE and MASTER_SITE_BACKUP.
checksum|	Verify that the fetched distfile's checksum matches the one the port was tested against.  Defining NO_CHECKSUM will skip this step.
depends|	Install (or compile if only compilation is necessary) any dependencies of the current port.  When called by the extract or fetch targets, this is run in piecemeal as fetch-depends, build-depends, etc.  Defining NO_DEPENDS will skip this step.
extract|	Expand the distfile into a work directory.
patch|	Apply any patches that are necessary for the port.
configure|	Configure the port.  Some ports will ask you questions during this stage.  See INTERACTIVE and BATCH.
build|	Build the port.  This is the same as calling the all target.
install|	Install the port and register it with the package system. This is all you really need to do.

## RPM spec

|name|from |to    |what the function is about
----|------|-------|--------------------------
%prep|	%_sourcedir|	%_builddir|	This reads the sources and patches in the source directory %_sourcedir. It unpackages the sources to a subdirectory underneath the build directory %_builddir and applies the patches.
%build|	%_builddir|	%_builddir|	This compiles the files underneath the build directory %_builddir. This is often implemented by running some variation of "./configure && make".
%check|	%_builddir|	%_builddir|	Check that the software works properly. This is often implemented by running some variation of "make test". Many packages don't implement this stage.
%install|	%_builddir|	%_buildrootdir|	This reads the files underneath the build directory %_builddir and writes to a directory underneath the build root directory %_buildrootdir. The files that are written are the files that are supposed to be installed when the binary package is installed by an end-user. Beware of the weird terminology: The build root directory is not the same as the build directory. This is often implemented by running "make install".
bin|	%_buildrootdir|	%_rpmdir|	This reads the files underneath the build root directory %_buildrootdir to create binary RPM packages underneath the RPM directory %_rpmdir. Inside the RPM directory is a directory for each architecture, and a "noarch" directory for packages that apply to any architecture. These RPM files are the packages for users to install.
src|	%_sourcedir|	%_srcrpmdir|	This creates a source RPM package (.src.rpm) inside the source RPM directory %_srcrpmdir. These files are needed for reviewing and updating packages.

### judicium

Apparently, the *pkgsrc* seems to be the most factorized build process.
It is worthy to check though if the naming convention suits EasyBuild purposes.
