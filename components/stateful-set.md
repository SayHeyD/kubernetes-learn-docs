[← Contents](../README.md)

# StatefulSet

StatefulSets are ReplicaSets which contain pods that run a stateful application. StatefulSets have been made for applciations like MySQL, MongoDB, PostgreSQL, etc. So basically any application that needs to be stateful.

StatefulSets manage and monitor which pods can write the shared persistent data.

## Usefulness

As provided by my learning resources using StatefulSets is way more difficult than using deployments. This is the reason why stateful applications are mostyl hosted outside of the k8s cluster. 