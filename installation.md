[← Contents](./README.md)

# Installation

For the purpouse to learn kubernetes i would be possible to just set up [minikube](https://kubernetes.io/docs/tasks/tools/). Instead I chose to install a kubernetes master with 1 node.

The setup was carried out as the root user on Ubuntu 20.04 v-servers.

## Preparation

To prepare the setup of both the master and node server we first need to install kubeadm, kubelet and kubectl and a container runtime. [Installation docs](https://kubernetes.io/docs/setup/production-environment/tools/kubeadm/install-kubeadm/)

### Container runtime

To run kubernetes there needs to be a container runtime present. In my case i chose to [install docker](https://docs.docker.com/engine/install/ubuntu/).

We need to configure docker to you systemd a s it's cgroup driver. To accomplish this we need to create the file ```/etc/docker/daemon.json``` with the content:

```json
{
	"exec-opts": ["native.cgroupdriver=systemd"]
}
```

or just execute the following command: ```printf '{\n\t"exec-opts": ["native.cgroupdriver=systemd"\n}\n' > /etc/docker/daemon.json```

afterwards we need to restart docker and kubelet: ```systemctl restart docker && systemctl restart kubelet```

## Master installation

After our preperation we just need to execute ```kubeadm init --pod-network-cidr 10.0.0.0/8``` to initialize our master node.

If the installation was successful we will be presented with a console output similar to:

```
Your Kubernetes control-plane has initialized successfully!

To start using your cluster, you need to run the following as a regular user:

  mkdir -p $HOME/.kube
  sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
  sudo chown $(id -u):$(id -g) $HOME/.kube/config

Alternatively, if you are the root user, you can run:

  export KUBECONFIG=/etc/kubernetes/admin.conf

You should now deploy a pod network to the cluster.
Run "kubectl apply -f [podnetwork].yaml" with one of the options listed at:
  https://kubernetes.io/docs/concepts/cluster-administration/addons/

Then you can join any number of worker nodes by running the following on each as root:

kubeadm join <IP_OF_DEVICE>:6443 --token <TOKEN_TO_JOIN>
```

Save the last printed out command or the value specified after ```--token``` somewhere. We need it later to join our worker node to our cluster. Notice that this token is only valid for 24 hours. After the token expires you will have to [create a new one](./cluster-management/bootstrap-tokens.md).

Execute: ```export KUBECONFIG=/etc/kubernetes/admin.conf```

To check if everything worked we can now try to list all of our nodes: ```kubectl get nodes```

You should now see that our master node has the status of ```NotReady```

To change this we need to install a network addon. In this case i chose fannel. We need to install it and restart our kubelet: ```kubectl apply -f https://raw.githubusercontent.com/coreos/flannel/master/Documentation/kube-flannel.yml && systemctl restart kubelet```

if we now execute ```kubectl get nodes``` again we should see that our master status has been updated to ```Ready```

## Worker installation

Installing our worker is way easier. We just need to execute the ```kubeadm join ...``` command printed out in the console earlier.

If the worker was registered successfully we should be presented with the following console output.

```
This node has joined the cluster:
* Certificate signing request was sent to apiserver and a response was received.
* The Kubelet was informed of the new secure connection details.

Run 'kubectl get nodes' on the control-plane to see this node join the cluster.
```

To verify that our node has been added to our cluster we will execute the ```kubectl get nodes``` command on our master node.

We should now see two nodes both with the status ```Ready```

## Troubleshooting

If you fucked up anything on any node, the command ```kubeadm reset && rm -rf /etc/cni/net.d && iptables -F && iptables -t nat -F && iptables -t mangle -F && iptables -X``` will reset the kubelet.

If you get the message ```[kubelet-check] The HTTP call equal to 'curl -sSL http://localhost:10248/healthz' failed with error: Get "http://localhost:10248/healthz": dial tcp 127.0.0.1:10248: connect: connection refused.``` during the execution of ```kubeadm init``` make sure you configured docker and restartet docker and kubelet after reconfiguring.

If you ever need to get more detailed information on something use the command ```kubectl describe```. It can be used for pods, nodes, deployments, etc.

* ```kubectl describe nodes``` shows information for all nodes
* ```kubectl describe node/<NODE_NAM>``` shows information for the specified node
* ```kubectl api-resources``` shows a list of all resources you can describe.
