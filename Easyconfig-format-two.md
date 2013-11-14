**Note: this documentation is for a feature in development, and is thus preliminary and subject to change.**

This page describes the v2.0 format for easyconfig files in detail. This format is supported since EasyBuild v1.x (**FIXME**),
and will become the only supported format starting with EasyBuild v2.0.

## Format outline

The easyconfig format consists of three parts, which are described below: a **header** consisting solely of comments,
a **Python header** that is

### Header

* first line: format version specification
* comments

Example:

```python
# EASYCONFIGFORMAT 2.0
# this is another version test
```

### Python header

* docstring (triple-quoted string): specifies author, maintainer, ...
* common parameters in (PEP8) Python syntax (exec'ed, with limited capabilities, i.e. only certain builtins are allowed)

Example:

```python
name = 'doesnotexist'

homepage = "http://www.example.com/"
description = "doesnotexist is bug free software"
docurl = "http://www.example.com/docs"

license = VeryRestrictive

# source tarball filename
sources = ['%(name)s-%(version)s.tar.gz']

# download location for source files
source_urls = ['http://ftpmirror.gnu.org/gzip']

# make sure the gzip and gunzip binaries are available after installation
sanity_check_paths = {'files': ["bin/gunzip", "bin/gzip"], 'dirs': []}
```

### Sections

* a set of version- or toolchain-specific sections
 * customizations that depend on particular version/toolchain
* sections are indicated by section markers, e.g. `[section1]`
* each section contains key-value statements, e.g. `== 1.2.3` (note: no quotes!)
* typicaly, a default section marked by `[DEFAULT]` is included

```
[DEFAULT]
versions=1.0.0,>= 0.5.0
toolchains=dummy > 5,GCC == 3.0.0,ictce < 1.0
[goolf > 1]
versions=1.5.0
[2.0.0]
toolchains=dummy > 6,GCC > 4
```

## Comparison with easyconfig format v1.0

* specification of format version
* structured documentation in docstring
* exec'ed part in Python syntax is significantly limited w.r.t. use of builtins (e.g., no 'import')
* sections are properly parsed, as opposed to v1.0 where the whole easyconfig file is 'parsed' by exec'ing it as Python code

