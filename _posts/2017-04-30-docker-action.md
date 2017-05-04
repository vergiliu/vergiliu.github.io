---
layout: post
title: "Docker in Action by Jeff Nickoloff"
date: 2017-04-30 22:22:22
tags: [books, reviews, 2017]
rating: 4
---

For some time I wanted to get a more thorough intro to Docker, and getting some sort of coherent thoughts - besides the obvious "read the docs" - is welcome.

I will skim over some of the background information, even though useful, and focus more on the overall commands and concepts, in some cases at least :)

##### `docker help` - always useful

##### `docker run` - start containers
- `--link src:dest` one way network dependencies between containers, e.g. mysql:database, where both are containers
- `--detach|-d` run in (or how I like to remember it) daemon mode :) much useful
- `--interactive|-i` and `--tty / -t` to run
- `--name <name> `useful when working with a small number of containers, to id them, must be unique
- `--read-only` the filesystem cannot be altered, mounted as read-only
- `--publish|-p localhost_port:container_port` port mapping between localhost_port and the application running in the container_port
- `--env|-e <name=val>` inject environment variable from host to container
- `--volume|-v <location>` - bind mount a local folder from _location_ to the container, remote folder will be replaced if present
    - format of <location> can be also given as src_local_folder:dest_container_folder:ro or rw, might need extra settings in the Docker application
- `--volumes-from <container>` - copy volume/location from given container
- `--entrypoint <command>` useful for _commit_, set default command to be executed when container is started, e.g. _git_, and then running docker run container help will call in fact _git help_
- `--rm` remove the container once it enters the "Exited" state
    - container can be in 1 of 4 states: running, paused, exited, restarting
- `--net none|bridge|joined|host` - none is disabling networking, bridge is default, no need to specify it; joined; open container, which has full access to host network
    - `--hostname --dns` can be specified
    - `--add-host name:ip_address` to map an IP address to a hostname
- set limits related to `--memory` or CPU time w/ `--cpu-shares` 
    - add and remove capabilities w/ `--cap-add` or `--cap-drop` for SELinux or AppArmor
    - similar can be done for `--device` or shared memory w/ `--ipc`
    - `-u user_A:user_B` user ID or user name mapping
- container can run with LXC instead of the (now current) _libcontainer_, by using `--exec-driver=lxc`
- output can be captured and used in other scripts, 128 bit hash of running instance when starting up is output to console
- containers can be automatically restarted, by using a _supervisor or init process_ which can monitor other processes in the container

##### `docker stop <name/id>` - stop container
    - `-f` flag will send SIGKILL instead of SIGHUP
- `docker rm <name/id>` - remove contaier from host
- `docker commit -a <sign_author_string> -m <message>` to create a image from a modified container
- docker tag can be used to assign tags to existing images
- docker inspect image to find out more details about the image

##### other commands
- `docker ps` - show running containers
- `docker restart <name>` - well, yeah
- `docker logs <container>` - show logs
- `docker exec <id> <command>` - run <command> in running container <id>, can be --name from before
- `docker create `
- `docker export --output <file>.tar <name>` - get a flattened copy of the current <name>
- `docker history <image>` - will give you all the changes done to the UFS

##### Notes:
- volumes represent a simple way to share images between containers or hosts
- there are 3 types of networking in case of containers: closed (no access), bridged (default) - communication goes through the Docker bridge virtual interface, and joined where the containers share the network stack
- if multiple settings need to be set, e.g. more ports mapped, duplicate the flag like --port 80:80 --port 8080:8080
- as maybe known, each docker image is or can be made up of more "layers"
    - an union file system (UFS) is made up of a stack of layers, this may negatively impact performance in some specific cases