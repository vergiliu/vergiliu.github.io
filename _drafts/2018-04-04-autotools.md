---
layout: post
title: "Autotools by John Calcotte"
date: 2018-04-04 20:18:05
tags: [books, reviews, 2018, safaribooks]
rating: na
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
