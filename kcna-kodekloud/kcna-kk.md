# KCNA - Kubernetes and Cloud Native Associate

### PODS
A pod is the smallest deployable unit in Kubernetes

`kubectl run nginx --image=nginx ` : Running a POd with nginx image hosted from dockerhub

`kubectl describe pod nginx` : Display information about the pod nginx

`kubectl get pods -o wide` : Display information about the running pods with richer out

#### PODS With Yaml

```yaml
kind - apiVersion: 
POD - v1
Service - v1
ReplicaSet - apps/v1
Deployment - apps/v1
```
```yaml
# pod-definition.yml
apiVersion: v1
kind: Pod
metadata:
  name: nginx
  labels:
    app: nginx

spec:
  containers:
    - name: nginx
      image: nginx

# kubectl create -f pod-definition.yml
# kubectl get pods
# kubectl describe pods
```

### REPLICASETS
A Replication Controller ensures that the specified number of PODS run at every moment it is the older version of ReplicaSet which up to date now.

- __ReplicationController__ : 
```yaml
rc-definition.yaml
apiVersion: v1
kind: ResplicationController
metadata:
  name: myapp-rc
  labels:
    app: myapp
    type: front-end
spec:
  replicas: 3
  template:
    metadata: 
      name: nginx-pod
      labels:
        app: nginx
    spec:
    containers:
      - name: nginx-container
        image: nginx

# kubectl create -f rc-definition.yaml
# kubectl get recplicationcontroller
# kubectl get pods
```
- __ReplicaSet__ : 
A recplica set is a process which monitors the pods based on the selector attached to it.

```yaml
replicaset-definition.yaml
apiVersion: apps/v1
kind: ReplicaSet
metadata:
  name: myapp-replicaset
  labels:
    app: myapp
    tier: front-end
spec:
  replicas: 3
  template:
    metadata: 
      name: nginx-pod
      labels:
        app: nginx
    spec:
    containers:
      - name: nginx-container
        image: nginx
  selector:
    matchLabels:
      tier: front-end

# kubectl create -f replicaset-definition.yaml
# kubectl get recplicasets
# kubectl get pods

# kubectl replace -f replicaset-definition.yaml
# kubectl scale --replicas=6 -f replicaset-definition.yaml
# kubectl scale --replicas=6 replicaset myapp-replicaset
```
