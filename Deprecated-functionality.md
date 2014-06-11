Some of the functionality that was available in previous EasyBuild versions is deprecated (or will be soon),
and should no longer be used.

This page presents an overview of deprecated functionality, together with available alternatives.

**In EasyBuild v2.0 we will drop the support for this deprecated functionality, forcing you to use the alternative
features instead.**

## Python compatibility

Python 2.4 compatibility is deprecated.

* _deprecated since:_ EasyBuild v1.14.0 (July'14)
* _no longer supported since:_ EasyBuild v2.0 (planned)
* _alternatives_: **upgrade to Python v2.6.x or v2.7.x**

Even since EasyBuild v1.0, the codebase has been Python 2.4 compatible. One reason for this is that EasyBuild was
being used on a daily basis on Scientific Linux 5, in which the Python 2.4.x is the system default.

With EasyBuild v2.0 we will drop support for Python 2.4, and only ensure compatibility with Python 2.6.x or a
more recent Python 2.x. This will enable us to also make the codebase compatible with Python 3.x, a feat that is very
difficult without dropping support for Python 2.4.


## EasyBuild configuration

Old-style configurating is deprecated.

* _deprecated since:_ EasyBuild v1.3.0 (Apr'13)
* _no longer supported since:_ EasyBuild v2.0 (planned)
* _alternatives:_ **new-style configuration**

Early versions of EasyBuild v1.x provided support for configuring EasyBuild via a _Python module_ that was automagically
executed when available.

Since EasyBuild v1.3 a safer and more consistent way of configuring EasyBuild is supported, which aligns the EasyBuild
command line, `$EASYBUILD_X` environment variables and key-value style configuration files.

More information about the new(er) and recommended configuration style, and the differences with the old-style
configuration is available on the [[wiki page on EasyBuild configuration|Configuration#legacy-configuration-deprecated]].

Note that the default path for the new-style configuration path is `$XDG_CONFIG_HOME/easybuild/config.cfg` (or
`$HOME/.config/easybuild/config.cfg` if `$XDG_CONFIG_HOME` is not set); the default path `$HOME/.easybuild/config.cfg`
that was in place since EasyBuild v1.3.0 is deprecated since v1.11.0 (Feb'14).


## Easyconfig parameters

The `software_license` easyconfig parameter will become **mandatory**; easyconfig files that do not provide a value for
this parameter (from a predefined list of supported values) will no longer work in EasyBuild v2.0.

A couple of easyconfig parameters have been renamed, for consistency reasons:

* **`buildopts`** replaces the deprecated `makeopts` _(since EasyBuild v1.13.0 (May'14))_
* **`prebuildopts`** replaces the deprecated `premakeopts` _(since EasyBuild v1.13.0 (May'14))_

Using the `shared_lib_ext` "magic" variable representing the extension for shared libraries (`.so` on Linux, `.dylib`
on OS X) is deprecated; the easyconfig constant `SHLIB_EXT` should be using instead _(since EasyBuild v1.5.0 (June'13))_.

**The deprecated easyconfig parameters can only be used until EasyBuild v2.0**.

After that, EasyBuild will throw an error
if it detects any of these are still being used in an easyblock and/or defined by an easyconfig file.


## EasyBuild API

#### Easyblocks API (`easybuild.framework.easyblock`)

Some changes were made to the easyblocks API:

* the return type of the `extra_options` static method has been changed to a **dictionary**, rather than a list of
  key-value tuples
* only the `ext_name`, `ext_version` and `src` template strings can be used in the `exts_filter` extension filter
  easyconfig parameter; using the `name` and `version` template strings is deprecated _(since EasyBuild v1.2.0 (Feb'13))_
* determining the location of Python modules representing easyblocks based on the software name is deprecated; EasyBuild
  must be able to determine the easyblock module path solely based on the name of the easyblock Python class _(since
  EasyBuild v1.4.0 (May'13))_

**Easyblocks not taking into account these changes will only be supported until EasyBuild v2.0.**


#### `easybuild.tools.modules` Python module

The API of the `easybuild.tools.modules` Python module has been changed extensively when implementing support for
alternative module naming schemes _(EasyBuild v1.8.0 (Oct'13))_:

* use of `modules` class variable and the `add_module`/`remove_module` methods is deprecated; modules should be
  (un)loaded using the `load` and `unload` methods instead
* the `mod_paths` and `modulePath` named arguments for the `run_module` method are deprecated; the class instance
  should be created with a specific list of module paths instead
* using the `Modules` class to obtain a class instance representing a modules tool interface is deprecated,;
  the `modules_tool` function should be used instead

Next to this, the `get_software_root` and `get_software_version` functions will only take `$EBROOTFOO` and
`$EBVERSIONFOO` environment variables into account starting with EasyBuild v2.0, as opposed to also considering
the `$SOFTROOTFOO` and `$SOFTVERSIONFOO` environment variables (which were set in modules generated by EasyBuild v0.x).
Likewise, adhering to the `$SOFTDEVELFOO` environment variables is deprecated.


#### Other changes

A number of functions and methods that are part of the EasyBuild framework API have been renamed, mainly for consistency
reasons:

* `source_paths()` (in `easybuild.tools.config`) replaces the deprecated `source_path()` (since EasyBuild v1.8.0 (Oct'13))
* `get_avail_core_count()` (in `easybuild.tools.systemtools`) replaces the deprecated `get_core_count()`
   _(since EasyBuild v1.9.0 (Nov'13))_
* `get_os_type()` (in `easybuild.tools.systemtools`) replaces the deprecated `get_kernel_name`
   _(since EasyBuild v1.3.0 (Apr'13))_
* the `det_full_ec_version` function available from `easybuild.tools.module_generator` replaces the deprecated
  `det_installversion` function that was available from `easybuild.framework.easyconfig.*` _(since EasyBuild v1.8.0
  (Oct'13))_

Some functions have moved to a different location:

* the `read_environment` function is now provided by the `easybuild.tools.environment` module, rather than by
  `easybuild.tools.config` or `easybuild.tools.utilities` _(since EasyBuild v1.7.0 (Sept'13))_
* the `modify_env` function is now provided by the `easybuild.tools.environment` module, rather than by
  `easybuild.tools.filetools` _(since EasyBuild v1.7.0 (Sep'13))_
* the `run_cmd`, `run_cmd_qa` and `parse_log_for_error` functions are now provided by the `easybuild.tools.run` module,
  rather than by `easybuild.tools.filetools` _(since EasyBuild v1.11.0 (Feb'14))_

The `get_log` function provided by the `easybuild.tools.build_log` module has been deprecated entirely,
no alternatives are provided (since none are needed).

**These functions and methods will only be available under their deprecated name/location until EasyBuild v2.0**.

After that, EasyBuild will throw an error if they're still being used (e.g., in easyblocks).