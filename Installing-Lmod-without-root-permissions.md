This short guide will show how to install Lmod (and Lua, on which it depends) on Linux, without requiring root permissions.


### Lua

Build and install Lua using the source tarball available in the Lmod SourceForge repository ([http://sourceforge.net/projects/lmod/files/](http://sourceforge.net/projects/lmod/files/)). This version is a lot easier to build, and already includes the required Lua modules. At the time of writing this relates to the `lua-5.1.4.5.tar.gz` tarball.

1. Download and unpack [lua-5.1.4.5.tar.gz](http://sourceforge.net/projects/lmod/files/lua-5.1.4.5.tar.gz/download).

2. Configure, build and install Lua in a custom prefix, e.g., `$HOME/lua`. Make sure you have `libreadline` and `ncurses` available on your system. The Lua binaries are statically linked to avoid problems with `libreadline` and/or `ncurses` modules that are loaded.

```bash
./configure --prefix=$HOME/lua
# make MYLDFLAGS='-static' # static linking, but yield issues with Lua modules: 
make # dynamic linking (default), yields issues with libreadline/ncurses modules
make install
```

3. Make sure the `lua` binary is available in your `$PATH` (pro tip: put this in your `.bashrc`):

```bash
export PATH=$HOME/lua/bin:$PATH
```

### Lmod

1. Download and unpack the latest available Lmod version, [Lmod-5.0.tar.bz2](http://sourceforge.net/projects/lmod/files/Lmod-5.0.tar.bz2/download) at the time of writing.

2. Configure, build and install Lmod build, in a custom prefix:
```bash
./configure --prefix=$HOME/lmod && make install
```