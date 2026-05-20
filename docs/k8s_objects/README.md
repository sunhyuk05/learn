# Kubernetes: Objects
- [Reference](#reference)
- [Description](#description)
- [`spec` and `status`](#spec-and-status)
- [Manifest](#manifest)
<hr>

### Reference
- [Objects In Kubernetes](https://kubernetes.io/docs/concepts/overview/working-with-objects/)

<hr>

### Description
Kubernetes objects are persistent entities that is used to represent the state of the cluster. Specifically, they can describe:
- What containerized apps are running (and on which nodes)
- The resources available to those apps
- The policies around how those apps behave

> this is your cluster's *desired state*.

<hr>

### `spec` and `status`

`spec`: description of the characteristics you want the resource to have (desired state)

`status`: current state of the object, supplied and updated by the K8s system and its components.
- Control plane continually and actively manages every object's actual state to match the desired state you supplied

A Deployment is an object that can represent an app running on your cluster. When you create the Deployment, a `spec` can be specified so that there are three replicas of the app running. K8s reads the Deployment spec and start three instances of your desired app. If any of those instances fail (a status change), the K8s system responds to the difference between spec and status by making a correction.

<hr>

### Manifest
Must provide the object spec (desired state), as well as basic information about the object.

Most often, you provide the information to `kubectl` in a file known as a manifest. By convention, these are YAML (or JSON) files.

`kubectl` conver the information from a manifest into JSON or another supported serialization format when making the API request over HTTP.

Below is an example of a Deployment:
```YAML
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
spec:
  selector:
    matchLabels:
      app: nginx
  replicas: 2 # tells deployment to run 2 pods matching the template
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx:1.14.2
        ports:
        - containerPort: 80

```

<hr>