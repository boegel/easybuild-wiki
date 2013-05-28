This short guide will show how to install Lmod (and Lua, on which it depends) on Linux, without requiring root permissions.

### Lua

1. Go to https://github.com/LuaDist/lua/tags and download the latest Lua version. At the time of writing, the latest available Lua version was 5.2.2, which can be downloaded [here](https://github.com/LuaDist/lua/archive/5.2.2.tar.gz). The remainder of these commands will assume Lua v5.2.2 is being installed, you may need to adjust them accordingly.

2. Configuring the Lua build requires CMake (2.8 or more recent). Specify a prefix to install Lua in:

```bash
cmake -DCMAKE_INSTALL_PREFIX=$HOME/lua -G 'Unix Makefiles'
```

3. Build Lua by running `make`.

4. Install Lua using `make install`.
