# Kubeadm Upgrade Guide

Before upgrading any node, drain the node and after the upgrade, mark the node as uncordon.

```sh
kubectl drain <node-name> --ignore-daemonsets
```

## Step 1: Display the Upgrade Plan

Run the following command to display the upgrade plan. This will list all the Kubernetes core components that need to be upgraded.

```sh
kubeadm upgrade plan
```

## Step 2: Upgrade Kubeadm

First, upgrade `kubeadm` to the desired version (replace `1.31.XX` with the actual version number).

```sh
sudo apt-get upgrade kubeadm=1.31.XX
```

Then, apply the upgrade using `kubeadm`.

```sh
kubeadm upgrade apply v1.31.XX
```

## Step 3: Upgrade Kubelet on Each Node

After upgrading `kubeadm`, you need to upgrade `kubelet` on each node.

```sh
sudo apt-get upgrade kubelet=1.31.XX
```

Restart the `kubelet` service to apply the changes.

```sh
sudo systemctl restart kubelet
```

## Step 4: Verify the Upgrade

Check the status of the nodes to ensure they are upgraded.

```sh
kubectl get nodes
```

## Step 5: Uncordon the Node

After verifying the upgrade, uncordon the node to make it schedulable again.

```sh
kubectl uncordon <node-name>
```

Follow these steps for each node in your cluster.
