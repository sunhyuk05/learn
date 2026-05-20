# Kubernetes: Pods
- [Reference](#reference)
- [Description](#description)
- [Patterns](#patterns)
- [Using Pods](#using-pods)
- [Working with Pods](#working-with-pods)
- [Resource Sharing and Communication](#resource-sharing-and-communication)
- [Static Pods](#static-pods)
<hr>

### Reference
- [Kubernetes Documentation](https://kubernetes.io/docs/concepts/workloads/pods/)
<hr>

### Description
> Pods are the smallest deployable units of computing that you can create and manage in Kubernetes.

A Pod is a group or one or more containers, with shared storage and network resources. It also shares a specification for how to run the containers. A Pod contains one or more application containers which are relatively tightly coupled.

<hr>

### Patterns
Two main patterns of running pods:

#### Run a single container
The most common k8s usecase where a Pod is a wrapper around a single container. K8s manages Pods rather than managin the container directly.

#### Run multiple containers that need to work together
A Pod encapsulates an application composed of multiple, tightly coupled containers with shared resources. These co-located containers form a single cohesive unit.

This is an advanced use case and should only be used in specific cases in which containers are tightly coupled.

<hr>

### Using Pods
The following is an example of a Pod which consists of a container running the image `nginx:1.14.2`.

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: nginx
spec:
  containers:
  - name: nginx
    image: nginx:1.14.2
    ports:
    - containerPort: 80

```

#### Managing Pods
Even in the case of a singleton Pods, you usually don't create the Pods directly. Instead, they are created using workload resources such as Deployment or Job. This is because Pods are designed as relatively ephemeral, disposable entities.
- Deployment: High level resource object that manages a replicated application
- Job: A finite or batch task that runs to completion

Each Pod is meant to run a single instance of a given application. If horizontal scaling is required, replication is used (multiple Pods with a single instance). Replicated Pods are usually created and managed as a group by a workload resource and its controller.

Pods natively provide two kinds of shared resources for their constituent containers: networking and storage.

<hr>

### Working with Pods
When a Pod gets created, the new Pod is scheduled to run on a Node in your cluser. The Pod remains on that node until the Pod finishes execution, object is deleted, is evicted for lack or resources, or the node fails.

<hr>

### Resource Sharing and Communication
#### Storage
A Pod can specify a set of shared storage volumes. All containers in the Pod can access the shared volumes. Volumes also allow persistent data in a Pod to survive in case one of the containers wthin needs to be restarted.

#### Networking
Each Pod is uniquely assigned an IP address. Every container in a Pod shares the network namespace, including the IP address and network ports.

Inside a Pod, the containers that belong to the Pod can communicate with one another using `localhost`. When communicating with outside entities, they must coordinate how they use the shared network resources.

<hr>

### Static Pods
Static Pods are managed directly by the kubelet daemon on a specific node, without the API server observing them. While most POds are managed by the control plane, kubelet directly supervises each static Pod.
- Kubelet: an agent that runs on each node in the cluster; ensures containers are running in pod.

Main use for static Pods is to run a self-hosted controle plane; using the kubelet to supervise the individual control plane components.

<hr>