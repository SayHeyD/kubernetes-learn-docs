[← Contents](../README.md)

# Pod

Pods are the smallest unit of k8s. Pods are an abstraction layer over containers so that you only have to interact with kubernetes and not a container runtime like docker, contaierd, etc.

Generally speaking one pod contains one container. It is possible for a pod the contain multiple containers but this is only used in specific cases, such as when an application needs a helper application.

Pods are considered emphemeral, which means pods can get destroyed easily. This destruction happens if f.ex. the applicaiton inside the pod crashes. A soon as a pod is destroyed kubernetes will try to replace the pod with another pod. Recreated pods will be assigned a new IP address, this is very inconvenient if soething has to communicate with this pod (f.ex. when the pod is running a database). To avoid the problems that arise with this kubernetes uses [Services](./service.md).

## Networking

Kubernetes provides a virtual network. Each pod is part of this network and gets is own IP address. Containers do not get IP addresses, only the pods they belong to. This means, that if a pod contains multiple containers, all of the containers inside the pod will be reachable on the same IP address. From what I've seen the virtual network mostly used is 10.0.0.0/8.