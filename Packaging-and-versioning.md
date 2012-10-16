### Packaging

EasyBuild is split up into three different repositories and packages:

* **easybuild-framework**: contains the EasyBuild framework, which is generic support for building and installing software
* **easybuild-easyblocks**: contains a collection of easyblocks, each of which adding support for a particular (set of) software
* **easybuild-easyconfigs**: contains a collection of easyconfigs (.eb files), each of which specifying the details of a particular version of a software package to build

The main motivation behind this is the development rate of each of these. We expect that the framework will only be updated occasionally, e.g. to add new features. The easyblocks will be updated more regularly as existing software support is enhanced (e.g. to support the latest versions of software) or support for new software is added. Finally, the easyconfigs will be update frequently as new versions of supported software become available, or when supported software has been built with an additional toolchain.

### Versioning

We implement the following versioning scheme for these different parts of EasyBuild. 

First, the major version of all three parts indicates the API version of the framework. The minor versions of the framework and easyblocks packages indicates feature updates of these packages, but are not related to each other. The version number for the easyconfigs packages consists of three numbers: the major version that indicates the API of the framework, the minor version that indicates the required version of the easyblocks, and an additional number indicating the release version of the easyconfig files.

Examples of compatible combinations of versions:

* **framework v1.0, easyblocks v1.1, easyconfigs v1.1.3**: first release of framework with this API v1, v1.1 of easyblocks (second release), 4th release of easyconfigs for easyblocks v1.1
* **framework v1.2, easyblocks v1.1, easyconfigs v1.1.5**: third release of framework with API v1 (features updates), still compatible with existing easyblocks, and additional easyconfigs added
* **framework v1.3, easyblocks v1.5, easyconfigs v1.5.2**: fourth release of framework with API v1, 6th release of easyblocks for that API version, with accompanying 3rd release of easyconfigs
* **framework v2.0, easyblocks v2.0, easyconfigs v2.0.0**: initial release of new API v2, with easyblocks and easyconfigs that have been updated accordingly