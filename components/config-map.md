[← Contents](../README.md)

# ConfigMap

Provides the possiblity for external configuration of running applications in pods. This is useful if you wnat to provide your containers with f.ex. a databse url. __CREDENTIALS SHOULD NOT BE STORED INSIDE OF CONFIG MAPS__. If you want to pass credentials to your applications use [secrets](./secrets.md).

## Usage inside pods

Data stored in a ConfigMap can be injected into your pod as environment variables or as a file. Just like [secrets](./secrets.md).