[← Contents](./README.md)

# Kubectl

Kubectl is a command line client for k8s. It is provided by kubernetes and offers the most possibilities as far as kubernetes clients go.

## Basic commands

### API Resources

API resources are the resources we can view and manipulate with the kubectl client. A complete list of theese resources can be viewd with ```kubectl api-resources```.

The [components](./components) are valid resources:

* [configmaps](./components/config-map.md)
* [deployments](./components/deployment.md)
* [ingresses](./components/ingress.md)
* [nodes](./components/node.md)
* [pod](./components/pod.md)
* [secrets](./components/secrets.md)
* [statefulsets](./components/stateful-set.md)
* [volumeattachments](./components/volumes.md)

### Show help

```bash
kubectl help
```

If you want to get more information about a single command shown with ```kubectl help```. You can execute ```kubectl <COMMAND> -h``` which will display inforamtion about the provided command and the options you have while using it.

### kubectl get

With ```kubectl get <RESOURCE>``` it is possible to view basic information about the provided resource f.ex.:

```bash
kubectl get nodes
```

Will show us all nodes and the status of them.

### kubectl create

With ```kubectl create <COMPONENT>``` you can create a new k8s component. This can be used if you f.ex. want to create a new service:

```bash
kubectl crate service <FLAGS> <OPTIONS>
```

Or create a deployment with the most basic options:

```bash
kubectl create deployment nginx-depl --image=nginx
```

In this case ```nginx-depl``` will be the name of our deployment and with ```--image=nginx``` we declare which image we want to execute in our [pods](./components/pod.md)

You cannot create [pods](./components/pod.md) with the create command.

### kubectl edit 

This command allows us to edit the provided resource.

```bash
kubectl edit <RESOURCE_TYPE> <RESOURCE_NAME>
```

If we want to edit a deployment with the name ```nginx-depl``` we would need to execute:

```bash
kubectl edit deployment nginx-depl
```

This will now open an auto-generated configuration file of the deployment in our default text-editor. The kubernetes configurations are written in YAML. After we save the file k8s will automatically update all the resources controlled by the edited resource, if this is needed.

### kubectl delete

This command allows us to delete a resource.

```bash
kubectl delete <RESOURCE_TYPE> <RESOURCE_NAME>
```

This will delete the resource and every other resource managed by that resource. If we want to delete a deployment with the name ```nginx-depl``` we would need to execute:

```bash
kubectl delete deployment nginx-depl
```

In this case everything that was owned by this deploymsnt will now be automatically deleted.

### kubectl logs

This command allows us to show us the logs of a specified pod.

```bash
kubectl logs <POD_NAME>
```

If we ant to get the log output of a pod named ```nginx-depl-5c8bf76b5b-ndqf4``` we would neeed to execute:

```bash
kubectl logs nginx-depl-5c8bf76b5b-ndqf4
```

### kubectl describe

This command is used for debugging or if ```kubectl get``` doesn't provide us with enough information. Describe will print out a detailed output of what you wna tto have described.

```bash
kubectl describe <RESOURCE>
```

If we would want to describe one single nginx deployment with the name ```nginx-depl``` we would execute:

```bash
kubectl describe deployment nginx-depl
```

This will provide us with information of only the ```nginx-depl``` instead of every running deployment.

### kubectl exec

This command is used if we want to get into the terminal of a pod. To start an interactive bash terminal inside a pod we can use:

```bash
kubectl exec -it <POD_NAME> -- bin/bash
```

To exit this iteractive terminal simply use the ```exit``` command.

### kubectl apply

With this command we can apply k8s configuration files which are written in YAML. This file can be stored locally or online. If we f.ex. want to start a nginx deployment with 3 replicas of a nginx pod we can use an example file of kubernetes.

```bash
kubectl apply -f https://k8s.io/examples/controllers/nginx-deployment.yaml
```