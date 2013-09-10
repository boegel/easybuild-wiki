This short guide will show how to install Lmod (and Lua, on which it depends) on Linux, without requiring root permissions.


### Lua

Build and install Lua using the source tarball available in the Lmod SourceForge repository ([http://sourceforge.net/projects/lmod/files/](http://sourceforge.net/projects/lmod/files/)). This version is a lot easier to build, and already includes the required Lua modules. At the time of writing this relates to the `lua-5.1.4.5.tar.gz` tarball.

1. Download and unpack [lua-5.1.4.5.tar.gz](http://sourceforge.net/projects/lmod/files/lua-5.1.4.5.tar.gz/download).

2. Adjust the `lua/src/Makefile.in` makefile template to enforce static linking of the `readline` and `ncurses` libraries (to avoid problems with `libreadline` and/or `ncurses` modules that are loaded), using the following patch:
```diff
diff -ru lua-5.1.4.5.orig/lua/src/Makefile.in lua-5.1.4.5/lua/src/Makefile.in
--- lua-5.1.4.5.orig/lua/src/Makefile.in	2011-02-18 19:49:01.000000000 +0100
+++ lua-5.1.4.5/lua/src/Makefile.in	2013-06-02 13:22:28.601496000 +0200
@@ -98,7 +98,7 @@
 	$(MAKE) all MYCFLAGS=
 
 linux:
-	$(MAKE) all MYCFLAGS="$(MYCFLAGS) -DLUA_USE_LINUX" MYLIBS="-Wl,-E -ldl @LIBS@ -lncurses"
+	$(MAKE) all MYCFLAGS="$(MYCFLAGS) -DLUA_USE_LINUX" MYLIBS="-Wl,-E -ldl -Wl,-Bstatic @LIBS@ -lncurses -Wl,-Bdynamic"
 
 macosx:
 #	$(MAKE) all MYCFLAGS=-DLUA_USE_MACOSX
```
This can be done by saving the patch above in a file named `lua-5.1.4.5_Makefile-static-linking.patch` **(beware of tabs when copy-pasting the patch!)**, and running the following command in the unpacked `lua-5.1.4.5` directory:
```bash
patch -p1 < lua-5.1.4.5_Makefile-static-linking.patch
```


3. Configure, build and install Lua in a custom prefix, e.g., `$HOME/lua`. Make sure you have `libreadline` and `ncurses` available on your system.
```bash
./configure --prefix=$HOME/lua && make && make install
```

4. Make sure the `lua` binary is available in your `$PATH` (pro tip: put this in your `.bashrc`):
```bash
export PATH=$HOME/lua/bin:$PATH
```

### Lmod

1. Download and unpack the latest available Lmod version, [Lmod-5.1.5.tar.bz2](http://sourceforge.net/projects/lmod/files/Lmod-5.1.5.tar.bz2/download) at the time of writing.

2. Configure, build and install Lmod build, in a custom prefix:
```bash
./configure --prefix=$HOME && make pre-install
```

3. Update `$PATH` so `lmod` is available (put this in your `.bashrc`):
```bash
export PATH=$HOME/lmod/5.1.5/libexec:$PATH
```