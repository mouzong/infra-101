# Certified Kubernetes Applciation Developer ( CKAD ) Study Notes

## 1 - Core Concepts
#### Docker vs ContainerD
- `imagespec` : SPecifications on how an image should be build
- `runtimespec` : specifications on how a container should be build.
- `dockershim` : Hacky way to support Docker outside the CRI
- `containerd` : containerD is an industry standard CRI which can now be installed as a standalone. It come with a command line tool called `ctr` specifically based for debugging in PROD.

- `nerdctl` : 
- `crictl` : Provides CLI for CRI compatible runtimes, it is developped and maintained by the kubernetes community. It is used to inspect and debug containers. It can be installed seperately. It is compatible with mostly all docker commands.

### Creating PODS using YAML definition file
```yaml
# pod-definition.yml

apiVersion: v1
kind: Pod
metadata:
  name: myapp-pod
  labels:
    app: myapp
    type: frontend

spec:
  containers:
    - image: nginx
      name: nginx-controller


```

### Replication Conttrollers

The replication controller ensures that a specified number of pods is running all the time.

```yml
rc-definition.yml
apiversion: v1
kind: ReplicationController
metatdata: 
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
        type: frontend
    spec:
      containers:
        - image: nginx
          name: nginx-controller
# kubectl create -f rc-definition.yml
# kubectl get replicationcontroller
    
```

### ReplicaSet
```yml
# replicaset-definition.yml
apiversion: apps/v1
kind: ReplicaSet
metatdata: 
  name: myapp-replicaset
  labels:
    app: myapp
    type: front-end
spec:
  selector:
    matchLabels:
      type: front-end
  replicas: 3
  template:
    metadata:
      name: myapp-pod
      labels:
        app: myapp
        type: frontend
    spec:
      containers:
        - image: nginx
          name: nginx-controller
# kubectl create -f replicaset-definition.yml
# kubectl get replicaset
# kubectl replace -f replicaset-definition.yml
# kubectl replace --replicas=6 -f replcaset-definition.yml
# kubectl scale rs --replicas=6 myapp-replicaset
```

## 2 - Configuration


## 3 - Multi-Container Pods

## 4 - Observability 

## 5 - Pod Design

## 6 - Services & Networking

## 7 - State Persistence 

## 8 - Security

## 9 - Helm Fundamentals

## 10 - Addition Practice - Kubernetes Challenge (Optional)