# Kubernetes Node Management

This repository contains commands for managing Kubernetes nodes, including draining, uncordoning, and cordoning nodes.

## Commands

### Draining a Node

To safely evict all pods from a node (except for DaemonSet-managed pods) and prepare it for maintenance, use the following command:

```sh
kubectl drain node01 --ignore-daemonsets
```

### Uncordoning a Node

To mark a node as schedulable and allow new pods to be scheduled on it, use the following command:

```sh
kubectl uncordon node01
```

### Cordoning a Node

To mark a node as unschedulable and prevent new pods from being scheduled on it, use the following command:

```sh
kubectl cordon node01
```

## Sample
### Deployment Example

This example demonstrates how to manage pods across two nodes (`node01` and `node02`) by draining, uncordoning, and deleting pods.

1. **Create a Deployment with Two Pods**

    First, create a deployment that runs two pods:

    ```sh
    kubectl create deployment my-deployment --image=nginx --replicas=2
    ```

2. **Drain `node01`**

    To prepare `node01` for maintenance, drain it:

    ```sh
    kubectl drain node01 --ignore-daemonsets
    ```

3. **Verify Pods are Running on `node02`**

    Check the status of the pods to ensure they are running on `node02`:

    ```sh
    kubectl get pods -o wide
    ```

4. **Uncordon `node01`**

    Mark `node01` as schedulable again:

    ```sh
    kubectl uncordon node01
    ```

5. **Delete a Pod on `node02`**

    Delete one of the pods running on `node02`:

    ```sh
    kubectl delete pod <pod-name>
    ```

    The deleted pod will be rescheduled on `node01`.

6. **Verify Pod Rescheduling**

    Check the status of the pods to verify that one pod is now running on `node01`:

    ```sh
    kubectl get pods -o wide
    ```

This process demonstrates how to manage pod scheduling across multiple nodes in a Kubernetes cluster.
