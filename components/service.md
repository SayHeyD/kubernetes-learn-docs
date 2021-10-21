[← Contents](../README.md)

# Service

A service is used to provide a static IP address for pods so that other pods can reach the applicaiton running inside the pods consistently even if the IPs of the pods get replaced.

When creating a service you differentiate between internal services and external services. Which type of service is created depends on the configuration you provide. If you configure an external service, the service will be reachable under the [node's](./node.md) IP address and the specified port of the service. If you want to provide access to a service via a TLS connection, k8s offers [ingress](./ingress.md)

## Load balancing

A services also acts as a weighted load balancer for the replicated pods that it contains. The load balancing works accross nodes and redirects traffic to the pod that currently has the lowset load.
Because it doesn't make much sense to configure all of this manually k8s provides us with [deployments](./deployment.md) and [statefulSets](./stateful-set.md).

## Exposing a service the outside

If we want to access a service from outside the cluster we have to declare this in the configuration of our service. This can be done by adding the ```type:``` property to the ```spec:``` property of the service configuration. An external service to access an nginx server could look like this:

```yaml
apiVersion: v1
kind: Service
metadata:
  name: ngninx-service
spec:
  selector:
    app: nginx
  type: LoadBalancer
  ports:
    - protocol: TCP
      port: 80
      targetPort: 80
```

Don't get confused. If you create an internal service the service still acts as a load balancer, we simply have to delcare it like this to make the service available externally. A complete list of types can be found in the [kubernetes documentation](https://kubernetes.io/docs/concepts/services-networking/service/#publishing-services-service-types).