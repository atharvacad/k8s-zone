# k8s-zone

This repository contains Kubernetes configurations for deploying NGINX servers. The configurations include deployments, config maps, and static pods.

## Project Structure

```
.gitignore
manifests/
    configmaps/
        nginx-configmaps-3-0.yaml
        nginx-configmaps-3-1.yaml
        nginx-configmaps-3-2.yaml
    deployments/
        nginx-deployment-1.yaml
        nginx-deployment-2-0.yaml
        nginx-deployment-2-1.yaml
        nginx-deployment-2-2.yaml
        nginx-deployment-2-3.yaml
        nginx-deployment-2-4.yaml
        nginx-deployment-2-5.yaml
        nginx-deployment-3.yaml
        nginx-deployment-4.yaml
    pods/
        nginx-static-1.yaml
README.md
```

## Commands Used

### Deployments

To apply a deployment configuration:
```sh
kubectl apply -f manifests/deployments/nginx-deployment-1.yaml
```

To delete a deployment:
```sh
kubectl delete -f manifests/deployments/nginx-deployment-1.yaml
```

### ConfigMaps

To create a ConfigMap:
```sh
kubectl apply -f manifests/configmaps/nginx-configmaps-3-0.yaml
```

To delete a ConfigMap:
```sh
kubectl delete -f manifests/configmaps/nginx-configmaps-3-0.yaml
```

### Static Pods

Static pods are managed directly by the kubelet on a specific node. To create a static pod, place the pod definition file in the `/etc/kubernetes/manifests` directory on the node.

## Environment Variables

The `nginx-deployment-3.yaml` file demonstrates how to use environment variables from ConfigMaps:
```yaml
env:
- name: USERNAME_AS_Regular_ENV
  value: "nginx-user-Regular-as-env"
- name: USERNAME_AS_SINGLE_ENV
  valueFrom:
    configMapKeyRef:
      name: day2-configmaps-3-1
      key: username_as_single_env
envFrom:
- configMapRef:
    name: day2-configmaps-3-0
```

## Taints and Tolerations

The `nginx-deployment-2-3.yaml` file demonstrates how to use taints and tolerations:
```yaml
tolerations:
- key: "app"
  operator: "Equal"
  value: "nginx"
  effect: "NoSchedule"
```

## Node Affinity

The `nginx-deployment-2-5.yaml` file demonstrates how to use node affinity:
```yaml
affinity:
  nodeAffinity:
    requiredDuringSchedulingIgnoredDuringExecution:
      nodeSelectorTerms:
      - matchExpressions:
        - key: size
          operator: In
          values:
          - Large
```
