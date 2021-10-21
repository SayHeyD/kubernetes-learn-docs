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
  superSecretUsername: ZnVycnlLaXR0ZW4K
  superSecretPassword: ZGVmaW5pdGVseU15UGFzc3dvcmQK
  ...
```

Note that the name and the keys inside of the data property can be picked freely. The data you provide on your custom data properties needs to be base64 encoded. If you forget to encode your data your other components won't be able to read your secrets correctly. To encode a string with base64:

**MacOS and Linux**

```bash
echo -n 'yourString' | base64 | cat -e
```

The secret type property needs to present. [These options are provided by defualt by k8s](https://kubernetes.io/docs/concepts/configuration/secret/#secret-types).

If you want to use secrets make sure you apply them **before** you create any other component that needs the secret to be present:

```bash
kubectl apply -f <YOUR_SECRETS_FILE>
```

## Is this really secure?

Kinda. Since these secret configuration files are stored on your server and hopefully nowhere else, they are secure in the context of the server. This means that if somebody would manage to break into your server they could read these files and probably guess, that the values are only encoded. K8s provides the possibility to [encrypt secrets](https://kubernetes.io/docs/tasks/administer-cluster/encrypt-data/) which is not enabled by default.