[← Contents](../README.md)

# Service

A service is used to provide a static IP address for pods so that other pods can reach the applicaiton running inside the pods consistently even if the IPs of the pods get replaced.

When creating a service you differentiate between internal services and external services. Which type of service is created depends on the configuration you provide. If you configure an external service, the service will be reachable under the [node's](./node.md) IP address and the specified port of the service. If you want to provide access to a service via a TLS connection, k8s offers [ingress](./ingress.md)