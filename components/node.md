[← Contents](../README.md)

# Node

A node is a server or v-server hostet anywhere, that is publically reachable.

## What nodes are used for

Nodes are used to run our pods, services, ingress controllers, etc. Since the purpose of a k8s cluster is to provide scalable, distributed services it doesn't make much sense to only run one worker node in a cluster. When using k8s it doesn't really make much sense without using replications of our services.