---
layout: post
title: "Docker in Action by Jeff Nickoloff"
date: 2017-06-31 22:22:22
tags: [books, reviews, 2017]
rating: na
---

For some time I wanted to get a more thorough intro to Docker, and getting some sort of coherent thoughts - besides the obvious "read the docs" - is welcome.

I will skim over some of the background information, even though useful, and focus more on the overall commands and concepts, in some cases:

- `docker help` - always useful
- `docker run` - start container
    - `--link src:dest` _not yet completely sure what it does ???_
    - `--detach / -d` run in (or how I like to remember it) daemon mode :) much useful
    - `--interactive / -i` and `--tty / -t` to run
    - `--name <name> `useful when working with a small number of containers, to id them, must be unique
    - `--read-only` the filesystem cannot be altered, mounted as read-only
    - `-p localhost_port:container_port` port mapping between localhost_port and the application running in the container_port
    - `--env / -e <name=val>` inject environment variable from host to container
    - `--rm` remove the container once it enters the "Exited" state
        - container can be in 1 of 4 states: running, paused, exited, restarting
    - if multiple settings need to be set, e.g. more ports mapped, duplicate the flag like --port 80:80 --port 8080:8080
    - output can be captured and used in other scripts, 128 bit hash of running instance when starting up is output to console
    - containers can be automatically restarted, by using a _supervisor or init process_ which can monitor other processes in the container
- `docker ps` - show running containers
- `docker restart <name>` - well, yeah
- `docker logs <container>` - show logs
- `docker exec <id> <command>` - run <command> in running container <id>, can be --name from before
- `docker create `
- `docker stop <name/id>` - stop container
    - `-f` flag will send SIGKILL instead of SIGHUP
- `docker rm <name/id>` - remove contaier from host
