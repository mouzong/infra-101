# Install Kubernetes Cluster with kubeadm

## Resources:
[ Vagrantfile for the cluster ](https://github.com/kodekloudhub/certified-kubernetes-administrator-course)

[ Documentation for cluster setup ](https://kubernetes.io/docs/setup/production-environment/tools/kubeadm/install-kubeadm/)



## 1 - Install kubeadm

```bash
# add kubernetes signing keys for repos
sudo apt-get update
# apt-transport-https may be a dummy package; if so, you can skip that package
sudo apt-get install -y apt-transport-https ca-certificates curl gpg

# If the directory `/etc/apt/keyrings` does not exist, it should be created before the curl command, read the note below.
# sudo mkdir -p -m 755 /etc/apt/keyrings
curl -fsSL https://pkgs.k8s.io/core:/stable:/v1.31/deb/Release.key | sudo gpg --dearmor -o /etc/apt/keyrings/kubernetes-apt-keyring.gpg


# This overwrites any existing configuration in /etc/apt/sources.list.d/kubernetes.list
echo 'deb [signed-by=/etc/apt/keyrings/kubernetes-apt-keyring.gpg] https://pkgs.k8s.io/core:/stable:/v1.31/deb/ /' | sudo tee /etc/apt/sources.list.d/kubernetes.list


sudo apt-get update
sudo apt-get install -y kubelet kubeadm kubectl
sudo apt-mark hold kubelet kubeadm kubectl

sudo systemctl enable --now kubelet

```

## 2 - Install Container Runtime Interface (Containerd)
````bash
# check the default init system before choosing the cgroup to install 
ps -p 1
sudo apt update
sudo apt install -y containerd

mkdir -p /etc/containerd

containerd config default

# /etc/containerd/config.toml
[plugins."io.containerd.grpc.v1.cri".containerd.runtimes.runc]
  ...
  [plugins."io.containerd.grpc.v1.cri".containerd.runtimes.runc.options]
    SystemdCgroup = true
```
