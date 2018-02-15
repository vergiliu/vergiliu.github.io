---
layout: post
title: "Autotools by John Calcotte"
date: 2018-03-03 22:22:22
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
