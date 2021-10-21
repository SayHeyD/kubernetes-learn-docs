[← Contents](../README.md)

# ConfigMap

Provides the possiblity for external configuration of running applications in pods. This is useful if you wnat to provide your containers with f.ex. a databse url. __CREDENTIALS SHOULD NOT BE STORED INSIDE OF CONFIG MAPS__. If you want to pass credentials to your applications use [secrets](./secrets.md).

## Usage inside pods

Data stored in a ConfigMap can be injected into your pod as environment variables or as a file. Just like [secrets](./secrets.md).

## Creating a configmap config file

To create a basic ConfigMap we need to create a YAML file with the following content:

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: mongodb-configmap
data:
  yourDataKey: yourDataValue 
```

Contrary to a [secret](secrets.md) we do not have to provide a type for our ConfigMap. Also our data doesn't need to be base64 encoded, instead it's written as plain text. You can add as many key-value pairs as you want and apply the file just like any other config file with:

```bash
kubectl apply -f you-config-file.yaml
```