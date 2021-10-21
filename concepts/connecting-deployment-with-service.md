[← Contents](../README.md)

# Connecting a deployment with a service

[Both configuration files](../configurations/connecting-deployment-with-service)

*This service will not be reachable externally*

## Labeling

To connect a deployment to a service we need to use labels and selectors. You can read up on them in the [YAML configurations files docs](../yaml-configuration-files.md).

In this example we have two files the [deployment](../configurations/connecting-deployment-with-service/nginx-service-deployment.yaml) and the [service](../configurations/connecting-deployment-with-service/nginx-service.yaml). In the deployment file we can see, that the deployment is being labeled in the ````metadata``` property:

```yaml
...
metadata:
  name: nginx-depl
  labels:
    app: nginx
spec:
...
```

And in the service configuration we can see the selector that is selecting the deployment with the same label:

```yaml
...
spec:
  selector:
    app: nginx
...
```

As you can see the pods and the deployment have the exact same label. This is needed for the service to work properly, as it actually connects to the pods and not the deployment itself. That said it also needs a reference to the deployment which is why we provide the deployment with it's own label.

Pod label:

```yaml
...
template:
  metadata:
    labels:
      app: nginx
...
```

## Networking

It is also important for the [service](../copmponents/service.md) to able to connect to the [pods](./pod.md) this is why we have to define a ```containerPort``` in the [pods'](../copmponents/pod.md) configuration and we have to define the ```targetPort``` in the [service's](../copmponents/service.md) configuration. These ports **MUST** match for the service to connect correctly to the pods.

**Pod configuration**

```yaml
...
template:
  metadata:
    labels:
      app: nginx
  spec:
    containers:
    - name: nginx
      image: nginx:1.16
      ports:
      - containerPort: 8080         # Here the port gets defined
```

**Service configuration**

```yaml
...
spec:
  selector:
    app: nginx
  ports:
    - protocol: TCP
      port: 80                      # This is the port where the service is accessible at
      targetPort: 8080              # This port has to match with the containerPort in the pod's configuration
```

## Applying the configuration files

If we now want to apply the the deployment and service to the cluster we need to execute the following command, expecting your working directory to be the root of this repository:

```bash
kubectl apply -f ./configurations/connecting-deployment-with-service/nginx-service-deployment.yaml && kubectl apply -f ./configurations/connecting-deployment-with-service/nginx-service.yaml
```

*Note that the deployment **must** be applied before the service is applied. If you do it in reverse the selector in the service configuration won't be able to find anything that matches it*

## Validation

Because or service isn't reachable externally we will have to find another way to validate. This can be done using the ```kubectl describe``` command as it provides us with all the information needed for the service.

First we describe our service and look out for the ```Endpoints``` listed in the output:

```bash
kubectl describe service nginx-service
```

Provided me with this information:

```
...
Endpoints:         172.17.0.3:8080,172.17.0.4:8080
...
```

Now we only need to find out if the Ip adresses of the pods match this. We could use the ```kubectl describe``` command again as it also would print out the IPs of the pods. Because this will clutter our screen just a tiny bit too much we will use the following command instead:

```bash
kubectl get pod -o wide
```

*Notice: the -o option sets the output format. All available formats can be viewed by using the -h option*

Which should print out somthing similar to this:

```
NAME                         READY   STATUS    RESTARTS   AGE   IP           NODE       NOMINATED NODE   READINESS GATES
nginx-depl-f4b7bbcbc-p52g6   1/1     Running   0          13m   172.17.0.3   minikube   <none>           <none>
nginx-depl-f4b7bbcbc-zrz69   1/1     Running   0          13m   172.17.0.4   minikube   <none>           <none>
```

If the pods' IP addresses match the Enpoint IPs the description of the service provided us with, we can consider our service and deployment as working.

## Deleting the service and deployment

This is accomplished wiht the ```kubectl delete``` command, again expecting you to be in the root directory of this repository:

```bash
kubectl delete -f ./configurations/connecting-deployment-with-service/nginx-service-deployment.yaml && kubectl delete -f ./configurations/connecting-deployment-with-service/nginx-service.yaml
```