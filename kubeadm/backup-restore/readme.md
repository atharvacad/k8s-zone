# Backup and Restore etcd

## 1. Verify etcd API Version

Before proceeding, ensure that the `ETCDCTL_API` environment variable is set to 3:

```bash
export ETCDCTL_API=3
```

## 2. Describe the etcd Pod

To get information about the etcd pod and its configuration, run:

```bash
kubectl -n kube-system describe pod -l component=etcd
```

This provides details such as etcd endpoints, client certificates, and keys, which are required for backup and restore.

## 3. Take an etcd Snapshot

Use the following command to take a backup of etcd:

```bash
etcdctl --endpoints=https://127.0.0.1:2379 \
  --cacert=<trusted-ca-file> --cert=<cert-file> --key=<key-file> \
  snapshot save <backup-file-location>
```
## 4. Restore etcd from a Snapshot

To restore etcd from a snapshot, follow these steps:

1. **Stop the etcd Pod**: First, stop the etcd pod to ensure that no changes are made during the restoration process.

```bash
kubectl -n kube-system delete pod -l component=etcd
```

2. **Restore the snapshot**:
    ```sh
    etcdctl --data-dir <data-dir-location> snapshot restore snapshot.db
    ```

    If `<data-dir-location>` is the same folder as before, delete it and stop the etcd process before restoring the cluster. Otherwise, change etcd configuration and restart the etcd process after restoration to have it use the new data directory.

3. **Update etcd Pod Configuration**:
    - First, change `/etc/kubernetes/manifests/etcd.yaml`'s `volumes.hostPath.path` for `name: etcd-data` to `<data-dir-location>`.

4. **Restart the etcd Pod**:
    - Execute:
    ```sh
    kubectl -n kube-system delete pod <name-of-etcd-pod>
    ```
    - Or restart the kubelet service:
    ```sh
    systemctl restart kubelet.service
    ```
These steps ensure that the etcd cluster is restored correctly and is running with the restored data.