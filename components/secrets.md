[← Contents](../README.md)

# Secrets

k8s secrets are used to store credentials. Contrary to [ConfigMaps](./config-map.md) secrets are stored base64 encoded. k8s secrets also provide built-in security features, which provide protection against unauthorized access. These security mechanisms and protection is not provided by the [ConfigMaps](./config-map.md).

## Usage inside pods

Data stored in a secret can be injected into your pod as environment variables or as a file. Just like [ConfigMaps](./config-map.md).

## Creating a secret configuration

To create a secret we can use the following template:

```yaml
apiVersion: v1
kind: Secret
metadata:
  name: superduper-superSecret
type: Opaque
data:
  superSecretUsername:
  superSecretPassword:
  ...
```

Not that the name and the keys inside of the data property can be picked freely. The secret type property needs to present. [These options are provided by defualt by k8s](https://kubernetes.io/docs/concepts/configuration/secret/#secret-types).