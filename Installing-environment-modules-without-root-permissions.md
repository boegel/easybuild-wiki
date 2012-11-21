This short guide will explain how to install the environment modules software package without root permissions, together with Tcl on which it depends.

### Tcl

1. Go to [[http://www.tcl.tk]] and download the latest Tcl sources. 

At the time of writing, the latest available Tcl version was 8.5.13, which can be downloaded [[here|http://prdownloads.sourceforge.net/tcl/tcl8.5.13-src.tar.gz]]. The remainder of these commands will assume Tcl v8.5.13 is being installed, you may need to adjust them accordingly.

2. Unpack the Tcl source tarball:

```bash
tar xfvz tcl8.5.13-src.tar.gz
```

3. Pick a location where you will install Tcl. It should be a directory you have write permissions on.
My suggestion would be to use something like `$HOME/.local/Tcl`.

4. Go to the `unix` subdirectory of the unpacked Tcl directory, and run the `configure` script using the `--prefix` option:

```bash
cd tcl8.5.13/unix
./configure --prefix=$HOME/.local/Tcl
```

If you're building Tcl and environment modules on Mac, you should run `configure` in the `tcl8.5.13/macosx` directory instead.

5.

### environment modules