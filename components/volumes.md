[← Contents](../README.md)

# Volumes

Volumes are used to store persistent data. Data inside pods is not persistent. Volumes are nothing else then physical storage that is attached to your pod. This physical storage can be a storage-medium on your node or it can be a remote, as long as it is reachable by the node. If a volume is setup pods that are getting restarted have access to this volume, which allows us to have persistent data.

K8s __does not__ manage data persistance. If you need backups from your data you will have to create these yourself.