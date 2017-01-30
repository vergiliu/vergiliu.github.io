---
layout: post
title: "Kubernetes Intro"
date: 2017-02-02 23:45:00
tags: [sites, kubernetes, k8s]
rating: 0
---
* minikube start
* some other stuff - xhyve is based on Mac built in VM layer - https://github.com/kubernetes/minikube/blob/master/DRIVERS.md#xhyve-driver
* to set `eval $(minikube docker-env)`
    * to unset `eval $(minikube docker-env) -u`
* to create a docker image `docker build -t hello-node:v1 .`

A Kubernetes Pod is a group of one or more containers
* view deployments and pods `kubectl get pods` or `kubectl get deployments`
    * `kubectl get events`
* expose Pod as Service to be visible from outside `kubectl expose deployment hello-node --type=LoadBalancer`
    * see it w/ `kubectl get services`
    * add it (only for minikube) `kubectl service hello-node`
    * follow logs `kubectl logs $POD -f` can be determine from `get pods`
* update container `docker build ...:v2` after updating file
    * update docker image `kubectl set image deployment/hello-node hello-node=hello-node:v2`
    * run your app again (?!?) `kubectl service hello-node`
* cleanup
    * `kubectl delete service hello-node`
    * `kubectl delete deployment hello-node`
* `minikube stop`
