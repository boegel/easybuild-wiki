**Note: this documentation is for a feature in development, and is thus preliminary and subject to change.**

This page describes the v2.0 format for easyconfig files in detail. This format is supported since EasyBuild v1.9 (**FIXME**),
and will become the only supported format starting with EasyBuild v2.0.

## Format outline

The easyconfig format consists of three parts, which are described below: a **header** consisting solely of comments,
a **Python header** that is

### Header

* first line: format version specification
* comments

### Python header

* docstring: author, maintainer, ...
* common parameters in (PEP8) Python syntax (exec'ed, with limited capabilities, i.e. only certain builtins are allowed)

### Sections

* set of version- or toolchain-specific sections
 * customizations that depend on particular version/toolchain
* each section contains key-value statements, e.g. `version = 1.2.3` (note: no quotes!)

## Comparison with easyconfig format v1.0

* specification of format version
* structured documentation in docstring
* exec'ed part in Python syntax is significantly limited w.r.t. use of builtins (e.g., no 'import')
* sections are properly parsed, as opposed to v1.0 where the whole easyconfig file is 'parsed' by exec'ing it as Python code

