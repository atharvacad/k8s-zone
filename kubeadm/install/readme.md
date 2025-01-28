## Installation of kubeadm

Follow these instructions to install `kubeadm` and set up a Kubernetes cluster.

### Step 1: Install kubeadm, kubelet, and kubectl

1. **Update the package list and install dependencies**:
    ```sh
    sudo apt-get update
    sudo apt-get install -y apt-transport-https curl
    ```

2. **Add the Kubernetes APT repository**:
    ```sh
    curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | sudo apt-key add -
    cat <<EOF | sudo tee /etc/apt/sources.list.d/kubernetes.list
    deb http://apt.kubernetes.io/ kubernetes-xenial main
    EOF
    ```

3. **Install kubeadm, kubelet, and kubectl**:
    ```sh
    sudo apt-get update
    sudo apt-get install -y kubelet kubeadm kubectl
    sudo apt-mark hold kubelet kubeadm kubectl
    ```
### Step 2: Create a Kubernetes Cluster

1. **Install containerd**:
    ```sh
    sudo apt update
    sudo apt install -y containerd
    ```

2. **Check the process status**:
    ```sh
    ps -s 1
    ```

3. **Ensure that the systemd driver is set up for cgroup**:
    - If `systemd` is set up, configure containerd.

4. **Create a directory for containerd configuration**:
    ```sh
    sudo mkdir -p /etc/containerd/
    ```

5. **Generate the default containerd configuration**:
    ```sh
    containerd config default | sudo tee /etc/containerd/config.toml
    ```

6. **Update the containerd configuration to use systemd cgroup**:
    ```sh
    sudo nano /etc/containerd/config.toml
    ```

    - Update the `SystemdCgroup` setting to `true`:
    ```toml
    [plugins."io.containerd.grpc.v1.cri".containerd.runtimes.runc.options]
      SystemdCgroup = true
    ```

7. **Restart the containerd service**:
    ```sh
    sudo systemctl restart containerd
    ```

8. **Initialize the Kubernetes cluster**:
    ```sh
    sudo kubeadm init --apiserver-advertise-address 10.0.0.179 --pod-network-cidr="10.244.0.0/16" --upload-certs
    ```

Follow these steps to set up your Kubernetes cluster using `kubeadm` and manage your nodes effectively.