# Kubernetes: Basics
- [Reference](#reference)
- [Description](#description)
- [High Level Architeture](#high-level-architecture)
- [Core Components](#core-components)
<hr>

### Reference
- [Kubernetes Documentation](https://kubernetes.io/docs/concepts/overview/#why-you-need-kubernetes-and-what-can-it-do)
<hr>

### Description
Kubernetes (a.k.a. **K8s**) is an open source platform designed to automate the deployment, scaling, and management of containerized applications. If Docker is the container, Kubernetes is the orchestrator that manages thousands of those containers across a cluster of servers.

In modern cloud environments running a single container is easy, but managing a distributed system is hard. Kubernetes solves:
- High availability: if a container crashes, K8s restarts it automatically
- Scalability: it scales your application up and down based on CPU or memory usage
- Service discovery: it gives containers their own IP addresses and a single DNS name for a set of containers
- Rollouts / rollbacks: you can update your code without downtime; if something goes wrong, it rolls back the change

<hr>

### High Level Architecture
Kubernetes operates on Leader-Worker (Control Plane and Node) model.

#### Control Plane
- `kube-apiserver`: front end; every command you run goes here
- `etcd`: key value store that acts as the cluster's source of truth (db)
- `kube-scheduler`: decides which worker node a new container should sit on
- `kube-controller-manager`: handles cluster level functions like noticing when a node goes down

#### Worker Nodes
- `kubelet`: an agent that runs on each node to ensure containers are running in a pod
- `kube-proxy` (optional): handles networking, ensuring traffic gets to the right container
- `Container runtime`: the software that runs the containers (e.g. Docker, containerd)

<hr>

### Core Components
In Kubernetes, you don't manage containers directly. Instead, you manage **Objects**:
- Pod: the smalles unit; a wrapper for one or more containers
- Service: an abstract way to expose an application running on a set of Pods as a network service
- Deployment: describes the "desired state" (e.g. "I want 3 copies of this app running")
- Namespace: a virtual cluster indside your physical cluster used to organize different projects

<hr>