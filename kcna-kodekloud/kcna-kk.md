# KCNA - Kubernetes and Cloud Native Associate

### PODS
A pod is the smallest deployable unit in Kubernetes

`kubectl run nginx --image=nginx ` : Running a POd with nginx image hosted from dockerhub

`kubectl describe pod nginx` : Display information about the pod nginx

`kubectl get pods -o wide` : Display information about the running pods with richer out

#### PODS With Yaml

```yaml
kind - apiVersion: 
POD         - v1
Service     - v1
ReplicaSet  - apps/v1
Deployment  - apps/v1
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
spec:
  selector:
    matchLabels:
      app: myapp
  replicas: 3
  template:
    metadata: 
      name: nginx
      labels:
        app: nginx
    spec:
      containers:
        - name: nginx
          image: nginx
  

# kubectl create -f replicaset-definition.yaml
# kubectl get recplicasets
# kubectl get pods

# kubectl replace -f replicaset-definition.yaml
# kubectl scale --replicas=6 -f replicaset-definition.yaml
# kubectl scale --replicas=6 replicaset myapp-replicaset
```
### DEPLOYMENTS

```yaml
depoy-definition.yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: myapp-deployment
  labels:
    tier: frontend
spec:
  template:
    metadata:
      name: nginx-2
      labels:
        app: myapp
    spec:
      containers:
        - name: nginx
          image: nginx
  replicas: 6
  selector:
    matchLabels:
      app: myapp

# kubectl run nginx --image=nginx
# kubectl get all
# kubectl apply -f deployment-patch.yaml
# kubectl create -f deployment.yaml
# kubectl get deployments
# kubectl set image deployment-name nginx=nginx:1.9.1
# kubectl rollout status deployement-name
# kubectl rollout history deployment-name
# kubectl rollout undo deployment-name
```

### Deployments: Rolling Updates and Rollbacks
when you create a deployment, a ROllout is created `Revision 1`
- `kubectl rollout status <deployment-name>`
- `kubectl rollout history <deployment-name>`

#### Deployment strategies
1 - Recreate
2 - Rolling update [DEFAULT] 

### Imperative & Declaratice approach in kubernetes
 The difference between imperative and declarative approach lies in the commands triggered directly from the terminal (imperative) and the commands packed into `.yaml` file which is triggered in bulk. 

 - `kubectl apply -f /path/to/config-files` : cerate objects from multiple fils in a directory at once in bulk
 - `kubectl expose deployment nginx --type="NodePort" --port 80` : Create a service to expose an nginx deployment

 ### Namespaces
 Namespaces in kubernets refer to logical divisions for data and resources isolations
 - `kubectl create namespace dev ` : Creating a new namespace called `dev`
 - `kubectl config set-context $(kubectl config current-context) --namespace=dev` : Setting the current namespace default to `dev` namespace.  

 - To limit resources in a namepace, we create a resource quota.

 ### Labels & Selectors
 - `kubectl get pods --selector app=myapp` : Get pods with the label app=myapp

 ### Taints and Tolerations
 - Taints are placed on Nodes and Tolerations are placed on pods to match the tolerations on the nodes, thereby making the scheduling of the pod on this node possible.
 - `kubectl taint nodes node-name key=value:tain-effect`
 - taint-effect = [NoSchedule | PreferNoSchedule | NoExecute]  
 - ex: `kubectl taint nodes node1 app=blue:NoSchedule`
 - Taint does not restrict a pod to be scheduled on a specific Node, instead it limits node to be tolerant to a certain category of Pods bearing `tolerations`

#### a - NoExecute Taint

 ```yaml
 # pod-definition.yaml
 apiVersion: v1
 kind: Pod
 metadata:
   name: myapp-pod
   labels:
     tier: fronend
 spec:
   containers:
     - name: nginx-container
       image: nginx

   tolerations:
     - key: "app"
       operator: "Equal"
       value: "blue"
       effect: "NoSchedule"
 # kubectl taint nodes node1 app=blue:NoSchedule
 # kubectl describe nodes node1 | grep -i Taint
 ```

### Labels 
- `kubectl label nodes node1 size=large` : Assigna label to a Node in the Cluster.
```yaml
 # pod-definition.yaml
 apiVersion: v1
 kind: Pod
 metadata:
   name: myapp-pod
   labels:
     tier: fronend
 spec:
   containers:
     - name: nginx-container
       image: nginx
   nodeSelector:
     size: large
   tolerations:
     - key: "app"
       operator: "Equal"
       value: "blue"
       effect: "NoSchedule"
 # kubectl taint nodes node1 app=blue:NoSchedule
 # kubectl describe nodes node1 | grep -i Taint
 ```

 ### Node Affinity
 - it provides advanced capabilities to limit the pod placement on specific nodes
 ```yaml
 # pod-definition.yaml
 apiVersion: v1
 kind: Pod
 metadata:
   name: myapp-pod
 spec:
   containers:
     - name: data-processor
       image: data-processor
   affinity:
     nodeAffinity:
       requiredDuringSchedulingIgnoredDuringExecution:
         nodeSelectorTerms:
           - matchExpressions:
               - key: size
                 operator: In | NotIn | Exists
                 values:
                   - Large
                   - Medium
 ```
#### Types of Node Affinity
- `requiredDuringSchedulingIgnoredDuringExecution`
- `preferredDuringSchedulingIgnoredDuringExecution`