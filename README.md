# Kubernetes

## Installation

Just to learn kubernetes i would be possible to just set up [minikube](https://kubernetes.io/docs/tasks/tools/). Instead I chose to install a kubernetes master with 1 node.

### Preparation

To prepare the setup of both the master and node server we first need to install kubeadm, kubelet and kubectl. [Installation docs](https://kubernetes.io/docs/setup/production-environment/tools/kubeadm/install-kubeadm/)

#### Container runtime

To run kubernetes there needs to be a container runtime present.Im my case i chose to [install docker](https://docs.docker.com/engine/install/ubuntu/).

### Master installation

### Node installation

### Troubleshooting

If you fucked up anything on any node, the command ```kubeadm reset``` will reset the kubelet.
