[← Contents](../README.md)

# Bootstrap tokens

Bootstrap tokens are needed to join a new node to the cluster. The full documentation can be found [here](https://kubernetes.io/docs/reference/setup-tools/kubeadm/kubeadm-token/).

## Listing tokens

To list all tokens execute: ```kubeadm token list```

This command will onyl list active tokens. Tokens that have been deleted or expired cannot be viewed with this command.

## Creating a token

To create a token with which you can add a node to the cluster execute: ```kubeadm token create```

If you want the output of this command to be a copy pastable command you can enter on the new node add the ```--print-join-command``` option. ```kubeadm token create --print-join-command```

The default TTL of created tokens is 24 hours.

## Deleting a token

To delete a token execute ```kubeadm token delete [token-value]```. After deleting a token it can be no longer used to join the cluster.