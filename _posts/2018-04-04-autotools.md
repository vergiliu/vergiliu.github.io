---
layout: post
title: "Autotools by John Calcotte"
date: 2018-04-04 20:18:05
tags: [books, reviews, 2018, safaribooks]
rating: 4
---

Make
- automatic variables
- $@ refers to the full target name of the current target
- $(MAKE) refers to main make
- '-' before commands ignores the exit code
- Targets are not always files. They can also be so-called phony targets, as in the case of all and clean.
```{bash}
objects =  display.o
display.o: display.c display.h
program: $(objects)
    gcc -g -O0 -o $@ $(objects)
fud: baz.c
    sources=baz.c; gcc -o fud $${sources}
all clean:
    cd src && $(MAKE) $@
    -rm $(distdir).tar.gz >/dev/null 2>&1
```
- for the `install` part you should use the OS provided tools, in this case `install -d $(prefix)/bin` in order to create folders
    - all variables defined outside the Makefile, are overriding the ones used in the Makefile
- `$${PWD}` references to $PWD which will be passed to make, if used inside the Makefile
- if you have `uninstall` targets, be careful so you don't delete existing folders/files
- get acquainted with FHS to better understand what things should go in which location
- support _prefix variables_ in your Makefile, especially if you make use of them
```{bash}
prefix          = /usr/local
bindir          = $(exec_prefix)/bin
export prefix
export bindir
```
    - users can then easily override them, e.g. `make prefix=/usr install`
- remember that `make CC=gcc3` is not the same as `CC=gcc3 make` as the 1st case is overriding Makefile variables, whereas the 2nd is just declaring shell variables
    - "in general, it's better to set make variables on the make command line"

Libtool
- if interface is updated, 2 versions of same library (shared) can be installed in the same location
- there are 3 possible forms for shared libraries references resolved by the linker
    - Dynamic Linking at Load Time - users can override the shared libraries by exporting LD_PRELOAD, case in which, the symbols can be referenced from a different pre-loaded library which might overwrite/replace functionality in the wished library - e.g. malloc with logging for troubleshooting memory allocation issues
    - Automatic Dynamic Linking at Load Time aka lazy-loading, where the process will stop if at the moment of using the library it cannot find its symbols
    - Manual Dynamic Linking at RUntime - where the programmer takes care of opening (dlopen) and loading the symbols (dlsym) of a shared library, and possibly handling errors in case things go wrong
- Libtool provides a shared library called ltdl - it provides an abstraction for manual dynamic linking between shared-library and non–shared-library platforms
    - "Remember that calling a routine in a static library has the same effect as linking the object code for the called routine right into the calling program. The called routine ultimately becomes an integral part of the calling binary image"
