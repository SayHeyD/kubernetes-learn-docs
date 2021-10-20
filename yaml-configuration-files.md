[← Contents](./README.md)

# YAML configuration files

These configuration files are used to store configurations for your k8s cluster. The files are written im YAML and can be loaded with the ```kubectl apply``` command. The configurations inside of these files can be deployments, services, really anything that k8s supports.

The in depth documentation for this can be found [here](https://kubernetes.io/docs/concepts/overview/working-with-objects/kubernetes-objects/).

## Required information

For k8s to be able to understand the instruction we give it there needs to be a basic amount of information inside the files we provide. K8s need at least these properties to be present:

```yaml
apiVersion: apps/v1
kind: <RESOURCE_TYPE>
metadata:
  name: <RESOURCE_NAME>
spec:
  ...
```

### Metadata

In the metadata section you are giving k8s 'metadata' about what you are wanting to do f.ex. what name the deployment you want to create should have.

### Specification

With the specification you specify what you wnat to create. This is done with the ```apiVersion``` and ```kind```.

Which ```apiVersion``` has to be used for what can be seen in [this table](https://matthewpalmer.net/kubernetes-app-developer/articles/kubernetes-apiversion-definition-guide.html#kubernetes-apiversion-table).

And with ```kind``` you specify what kind of [component](./components/) you want to create.

### Spec

In the ```spec``` section you can define every property for your component. In this section you can f.ex. define how many replicas your deployment should have. What you can configure within this spec section is unqiue for every existing [component](./components).

## What happens when applying a configuration

K8s adds a status to your configuration. This does not have to be provided by you, this is managed by k8s and will automatically added to your configuration files once they're applied. With ```kubectl edit``` you can see what k8s writes into the status section. This status comes into play when you want to apply your configuration file again with some changes. Then k8s will compare what the desired and actual status of your configuration is. K8s needs to do this to be able to enforce the changes you wnat without completly recreating your configuration. You do not have to provide a status inside in a configuration file you have already applied.

## Where to store configuration files

A common practice for storing configuration files it to store it with your code. This way you exactly know which configuration belongs to which codebase.

Another option is to create a repository for only your configuration file.

These configuration files are considered as [IaaS](https://en.wikipedia.org/wiki/Infrastructure_as_a_service).It only bears advantages to have these files stored in a version controlled environment as it is way easier to rollback breaking cahnges this way.