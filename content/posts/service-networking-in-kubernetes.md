+++
author = "Sitesh Pattanaik"
title = "Service Networking in Kubernetes"
date = "2025-05-11"
description = "Understand how networking work with the perspective of the Service construct in kubernetes"
tags = [
"emoji",
]
+++

## Basics First
* Services are a construct provided by K8 to access different services running in pods
* Pod is localized to a node, but a service running in a pod if exposed via the Service construct is visible across the cluster
* Services are a virtual concept, there is no actual object
* `kube-proxy`, the process running in each node is responsible for implementing it

### Types of Services

#### ClusterIP
* These type of services are only accessible from within the cluster using "name" or "IP".

#### NodePort
* In addition to this service being accessed by IP, these types of services can be access from outside the cluster as well.

## How are these services getting IP and are accessible

kubelet
* We have 3 nodes running with each of them having an IP 192.168.1.1, 192.168.2.1, 192.168.3.1 respectively.
* Each node has a "kubelet" process running for creating the pods
* The `kubelet` process watches the state of the application and launches new pods as necessary
* The `kubelet` process talks to the CNI plugin to configure the networking for the pod

kube-proxy
* Similar to that of `kubelet`, `kube-proxy` is another agent that runs on each of the pod
* It monitors the creation of any service and helps implement that.
* The "Service" in the context of kubernetes is a virtual object, there isn't any server or service listening on to that port
* When a service is created, `kube-proxy` finds out the free IP from the range of pre-defined IPs for services and creates a rule in each of the node.
* kube-proxy configures the IP tables in each of the nodes, adding entries like "a.b.c.d:port" to that of the pod on which the service is actually running.

So, that is how the services get an IP and they are accessible from outside as well as across the cluster.