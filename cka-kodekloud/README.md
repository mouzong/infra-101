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