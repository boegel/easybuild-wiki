This short guide will show how to install Lmod (and Lua, on which it depends) on Linux, without requiring root permissions.

### Lua

### LuaDist

First, let's install the `luadist` tool, a convenient way to install Lua and Lua modules:

1. Clone the LuaDist git repository:

```bash
mkdir LuaDist
cd LuaDist
git clone https://github.com/LuaDist/Repository.git
cd Repository
cat .gitmodules | sed 's@git://@https://@g' > /tmp/gitmodules; mv /tmp/gitmodules .gitmodules
cat .gitmodules >> .git/config 
git submodule update --init bootstrap lua lua-git luadist-git luafilesystem luasocket srlua zlib
./install bootstrap
```



1. Go to https://github.com/LuaDist/lua/tags and download the latest Lua version. At the time of writing, the latest available Lua version was 5.2.2, which can be downloaded [here](https://github.com/LuaDist/lua/archive/5.2.2.tar.gz). The remainder of these commands will assume Lua v5.2.2 is being installed, you may need to adjust them accordingly.

2. Configuring the Lua build requires CMake (2.8 or more recent). Specify a prefix to install Lua in:
```bash
cmake -DCMAKE_INSTALL_PREFIX=$HOME/lua -G 'Unix Makefiles'
```

3. Build Lua by running `make`.

4. Install Lua using `make install`.

5. Make sure the `lua` binary is available in your `$PATH` (pro tip: put this in your `.bashrc`):

```bash
export PATH=$HOME/lua/bin:$PATH
```

### Lmod

1. Go to https://github.com/TACC/Lmod/tags and download the latest available Lmod version (v5.0rc4 at the time of writing).

2. Configure the Lmod build, while specifying a prefix to Lmod in:
```bash
./configure --prefix=$HOME/lmod
```