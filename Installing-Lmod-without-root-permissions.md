This short guide will show how to install Lmod (and Lua, on which it depends) on Linux, without requiring root permissions.


### Lua

Build and install Lua using the source tarball available in the Lmod SourceForge repository ([http://sourceforge.net/projects/lmod/files/](http://sourceforge.net/projects/lmod/files/)). This version is a lot easier to build, and already includes the required Lua modules. At the time of writing this relates to the `lua-5.1.4.8.tar.gz` tarball.

**Step 1**: Download and unpack [lua-5.1.4.8.tar.gz](http://sourceforge.net/projects/lmod/files/lua-5.1.4.8.tar.gz/download).

**Step 2**: Configure the Lua build, provide a custom installation prefix (e.g. `$HOME/lua`) and specify to statically link libraries (i.e. `libreadline` and `ncurses`), to avoid problems when modules that provide these libraries are being loaded. Then build and install via `make`:
```bash
./configure --with-static=yes --prefix=$HOME/lua && make && make install
```

**Step 3**: Make sure the `lua` binary is available in your `$PATH` (only required when building Lmod, see below):
```bash
export PATH=$HOME/lua/bin:$PATH
```

Optionally, check whether the `lua` binary indeed doesn't link to any `readline` or `ncurses` libraries:
```bash
$ ldd $HOME/lua/bin/lua
        linux-vdso.so.1 (0x00007fffad7ff000)
        libm.so.6 => /lib64/libm.so.6 (0x00007ff9914db000)
        libdl.so.2 => /lib64/libdl.so.2 (0x00007ff9912d7000)
        libc.so.6 => /lib64/libc.so.6 (0x00007ff990f2a000)
        /lib64/ld-linux-x86-64.so.2 (0x00007ff9917d9000)
```

### Lmod

**Step 1**: Download and unpack the latest available Lmod version, [Lmod-5.6.3.tar.bz2](http://sourceforge.net/projects/lmod/files/Lmod-5.6.3.tar.bz2/download) at the time of writing.
```bash
tar xfvj Lmod-5.6.3.tar.bz2 && cd Lmod-5.6.3
```

**Step 2**: Configure, build and install Lmod build, in a custom prefix:
```bash
./configure --prefix=$HOME && make pre-install
```

**Step 3**: Update `$PATH` so `lmod` is available (put this in your `.bashrc`):
```bash
export PATH=$HOME/lmod/5.6.3/libexec:$PATH
```

Optionally, give it a spin:
```bash
$ lmod --version

Modules based on Lua: Version 5.6.3 (5.6.3-19-g5e11bfa) 2014-06-23 05:28
    by Robert McLay mclay@tacc.utexas.edu
```

**Step 4**: Define `module` function to use `lmod` (optional for use with EasyBuild):
```bash
source $HOME/lmod/5.6.3/init/bash
export LMOD_CMD=$HOME/lmod/5.6.3/libexec/lmod
```
Or, alternatively, perform a full installation by running `make install` rather than `make pre-install`.