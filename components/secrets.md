[← Contents](../README.md)

# Secrets

k8s secrets are used to store credentials. Contrary to [ConfigMaps](./config-map.md) secrets are stored base64 encoded. k8s secrets also provide built-in security features, which provide protection against unauthorized access. These security mechanisms and protection is not provided by the [ConfigMaps](./config-map.md).

## Usage inside pods

Data stored in a secret can be injected into your pod as environment variables or as a file. Just like [ConfigMaps](./config-map.md).