# Certified Kubernetes Administrator - CKA

## 1 - Core Concepts

### - Kubernetes Architecture

![Kubernetes Architecture](img/k8s-architecture.png)

A kubernetes cluster is composed of multiple nodes amongst which we have the master and worker nodes
- `Master Node -` The master node is made up of :  
  - ETCD : Whoch key-value pair database whoch store information about the cluster
  - kube-apiserver : Orchestrates all operation within the cluster
  - kube-scheduler : schedules applications or pods on worker nodes
  - kube controller manager : Controls all fonctions like replication controller, node controller

- `Worker node -` On the other hand we have :
  - kubelet : Listens for instructions from the kube-apiserver and manages containers
  - kube-proxy : Enables communications beyween services in the cluster

### - ETCD

ETCD is a distributed reliable key-value store that is simple, secure and fast. </br>

To install ETCD you need to erform 3 actions:

```bash
# 1 - Download the binaries
curl -L https://github.com/etcd-io/etcd/releases/download/v3.5.16/etcd-v3.5.16-linux-amd64.tar.gz -o etcd-v3.5.16-linux-amd64.tar.gz

# 2 - Extract it
tar xvzf etcd-v3.5.16-linux-amd64.tar.gz

# 3 - Run th executable files
cd etcd-v3.5.16-linux-amd64
./etcd
```

ETCD runs by default on port `2379`

In kubernetes ETCD stores information about :
- Nodes
- PODs
- Configs
- Secrets
- Accounts
- Roles
- Bindings etc

#### - ETCD Commands
Additional information about ETCDCTL Utility
</br>ETCDCTL is the CLI tool used to interact with ETCD.ETCDCTL can interact with ETCD Server using 2 API versions – Version 2 and Version 3. By default it’s set to use Version 2. Each version has different sets of commands.

For example, ETCDCTL version 2 supports the following commands:

```bash
etcdctl backup
etcdctl cluster-health
etcdctl mk
etcdctl mkdir
etcdctl set
```

Whereas the commands are different in version 3

```bash
etcdctl snapshot save
etcdctl endpoint health
etcdctl get
etcdctl put
```

To set the right version of API set the environment variable ETCDCTL_API command

```bash
export ETCDCTL_API=3
```

When the API version is not set, it is assumed to be set to version 2. And version 3 commands listed above don’t work. When API version is set to version 3, version 2 commands listed above don’t work.

Apart from that, you must also specify the path to certificate files so that ETCDCTL can authenticate to the ETCD API Server. The certificate files are available in the etcd-master at the following path. We discuss more about certificates in the security section of this course. So don’t worry if this looks complex:

```bash
--cacert /etc/kubernetes/pki/etcd/ca.crt
--cert /etc/kubernetes/pki/etcd/server.crt
--key /etc/kubernetes/pki/etcd/server.key
```
So for the commands, I showed in the previous video to work you must specify the ETCDCTL API version and path to certificate files. Below is the final form:


```bash
kubectl exec etcd-controlplane -n kube-system -- sh -c "ETCDCTL_API=3 etcdctl get / --prefix --keys-only --limit=10 --cacert /etc/kubernetes/pki/etcd/ca.crt --cert /etc/kubernetes/pki/etcd/server.crt --key /etc/kubernetes/pki/etcd/server.key"
```

### ReplicaSet and Replication Controller
A replication controller ensures that nomber of pods is always runnig for a given Pod template. It is the old way of representing the replication. As of now the new way of ensuring the replication of Pods is the implementation of ReplicaSet

```yaml
# rc-definition.yaml
apiVersion: v1
kind: ReplicationController
metadata:
  name: myapp-rc
  labels:
    app: myapp
    type: front-end

spec:
  replicas: 3
  template:
    metadata:
      name: myapp-pod
      labels:
        app: myapp
        type: front-end
    spec:
      containers:
      - name: nginx-container
        image: nginx

# kubectl create -f rc-definition.yaml
```
The ReplicaSet works on the same basis as the ReplicationController, the sole difference between them is tha in the ReplicaSetwe have `selector:` which makes the matching of the labels between the ReplicaSet and the Pods it is monitoring.
```yaml
# rs-definition.yaml
apiVersion: apps/v1
kind: ReplicaSet
metadata:
  name: myapp-rs
  labels:
    app: myapp
    type: front-end

spec:
  replicas: 3
  template:
    metadata:
      name: myapp-pod
      labels:
        app: myapp
        type: front-end
    spec:
      containers:
      - name: nginx-container
        image: nginx
  selector:
    matchLabels:
      type: front-end

# kubectl create -f rs-definition.yaml
```