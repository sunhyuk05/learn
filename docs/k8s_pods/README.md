# Kubernetes: Pods
- [Reference](#reference)
- [Description](#description)
- [Patterns](#patterns)
- [Using Pods](#using-pods)
- [Working with Pods](#working-with-pods)
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