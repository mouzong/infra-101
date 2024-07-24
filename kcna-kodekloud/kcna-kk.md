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

### RESOURCE LIMITS
```txt
- 1 CPU : 1 AWS vCPU | 1 GCP Core | 1 Azure Core | 1 Hyperthread

- 256 Mi : 
```

### DAEMON SET
A DaemonSet ensures there is always an instance of a POD running on every node in the cluster
```yaml
# daemon-set-definition.yaml
apiVersion: apps/v1
kind: DaemonSet
metadata:
  name: monitoring-daemon
spec:
  selector:
    matchLabels:
      app: monitoring-agent
  template:
    metadata:
      labels:
        app: monitoring-agent
    spec:
      containers:
        - name: monitoring-agent
          image: monitoring-agent

# kubectl create -f daemon-set-definition.yaml
# kubectl get daemonsets
```

### Multiple Schedulers
```yaml
# my-scheduler-2-config.yaml
apiVersion: kubescheduler.config.k8s.io/v1
kind: KubeSchedulerConfiguration
profiles:
- schedulerName: my-scheduler-2
```

```yaml
# my-scheduler-config.yaml
apiVersion: kubescheduler.config.k8s.io/v1
kind: KubeSchedulerConfiguration
profiles:
- schedulerName: my-scheduler
```

```yaml
# scheduler-config.yaml
apiVersion: kubescheduler.config.k8s.io/v1
kind: KubeSchedulerConfiguration
profiles:
- schedulerName: default-scheduler
```

To deploy an additional scheduler you need to run a new Pod containing the definition for this new Scheduleras seen below:

```yaml
# my-scheduler-config.yaml
apiVersion: kubescheduler.config.k8s.io/v1
kind: KubeSchedulerConfiguration
profiles:
- schedulerName: my-scheduler
leaderElection:
  leaderElect: true
  resourceNamespace: kube-system
  resourceName: lock-object-my-scheduler
```

```yaml
# my-custom-scheduler.yaml
apiVersion: v1
kind: Pod
metadata:
  name: my-custom-scheduler
  namespace: kube-system
spec:
  containers:
  - command:
    - usr/local/bin/kube-scheduler
    - --address=127.0.0.1
    - --kubeconfig=/etc/kubernetes/scheduler.conf
    - --config=/etc/kubernetes/my-scheduler-config.yaml

    image: k8s.gcr.io/kube-scheduler-amd:v1.11.3
    name: kube-scheduler
```

You can check the kubernetes documentation for more information. after setting up the scheduler correctly we then setup a pod to be scheduled with a `schedulerName:`

## Container Orchestration - Security
### Security - Kuberntes Security Primitives
- Securing hosts mean disabling password based authentication, and allowing only SSH based authentication
- Authentication : Who can acces the cluster : 
- Authorization : what can authenticated users do when they access the cluster ?
- All communication between the kubeapi-server and cluster components is secured using TLS Certificates

### Authentication in K8S Cluster
- You cvan not create USer usng the k8s api but instead you can create Service Accounts.
-vAuth mechanisms in kubernetes involve the use of `static Password File`, `Static Token File`, `Certificates`, `Identity Services`
#### Static Password File
If you have this user details lists and you want to add them as authorized users on the cluster you need to creats a Static passwod file which is then passed to the `kube-apiserver.service` definition file under the `--basic-auth-file=user-details.csv`. <br>

`user-details.csv : `
```csv
password123,user1,u0001
password123,user2,u0002
password123,user3,u0003
```
To authenticate using user details in curl commend command, use the command below:

`curl -v -k https://master-node-ip:6443/api/v1/pods -u "user1:password123"`

Similarly with the Static Password file we can authenticate using a Static Token File defined like sen below.


```csv
2ada2aa0da79c3c73ab1423731a9bbf7,user1,u0001
425180db1907b0c046e9a4e08554d074,user2,u0002
df7d303c45e6c5e2a1fa95c1c947f50e,user3,u0003
```

For authentication add the `--token-auth-file=user-details.csv` to the kube-apiserver definition service then use the curl request below to authenticate.

`curl -v -k https://://master-node-ip:6443/api/v1/pods --header "Authorization: Beare df7d303c45e6c5e2a1fa95c1c947f50e"`. Add the token to your requests.

#### TLS in Kubernetes - certificate Creation
To generate certifcates in kube cluster we will make use of the OPENSSL tool. <br>

First thing to do is to setup the Certificate Authority CA
- We start by generating a private key : `openssl genrsa -out ca.key 2048`

- We then generate a certificate signing request : `openssl requ -new -key ca.key -subj "/CN=KUBERNETES-CA" -out ca.csr`

- we now sign the certificate : `openssl x509 -req in ca.csr -signkey ca.key -out ca.crt`

- We now genrate the client key for asmin users:
  - `openssl genrsa -out admin.key 2048`
  - `openssl req -new -key admin.key -subj "/CN=kube-admin" -out admin.csr`
  - `openssl x509 -req in admin.csr -CA ca.crt -CAkey ca.key -out admin.crt`
#### API Groups
- ``/metrics``
- ``/healthz``
- ``/logs``
- ``/version``
- ``/api`` : Core group
- ``/apis`` : Nmamedd group

#### Role Based Access Control
```yaml
# developer-role.yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: Role
metadata:
  name: developer
rules:
- apiGroups: [""]
  resources: ["pods"]
  verbs: ["list", "get", "create", "update", "delete"]
- apiGroups: [""]
  resources: ["Configmap"]
  verbs: ["create"]
```

After creating a Role you need to create a Role Binding object to attacht the newly created role to the user.
```yaml
# devuser-developer-binding.yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: Role
metadata:
  name: devuser-developer-binding
subjects:
- kind: User
  name: dev-user
  apiGroup: rbac.authorization.k8s.io
roleRef:
  kind: Role
  name: dev-user
  apiGroup: rbac.authorization.k8s.io

# kubectl get roles
# kubectl get rolebindings
# kubectl describe role developer
```

You as a user you can check your permissions on the cluster like :
- `kubectl auth can-i create deployments`

#### Service Accounts
There are two type of accounts in Kubernetes :
- `user account` : User | Admin | Developer
- `service account` : Jenkins | prometheus

- To create a Service Account with use the commend: `kubectl create serviceaccount dashboard-sa`. 
- After a SA is created it generates a token object as well (in older versions of kubernetes).

By default there is a SA for each namepace in the kubernetes Cluster and when you create a POD in the namespace,  you can the token inside the POD mounted at `/var/run/secrets/kuberntes.io/serviceaccount/token. <br>

To decode the token and fin the finformation about it you can simply use the command :
- `jq -R 'split(".") | select(length > 0) | .[0],.[1] | @base64d | fromjson' <<< eyJhbGci0iJ....`

#### Securing Images
HWe working with images in a cluster we need to specify the image registry which we will want to use as source for our container build. for that we need to login to registery and then use the fqdn of the images in the manifest files.

When using a local computer we proceed as follows:

- Login first : `docker login private-registry.io`
- Run container : `docker run private-registry.io/apps/internal-app`

In the kubernetes cluster you need to create a secert object for the cluster to authenticate against when pulling imges to run the PODS.

```bash
kubectl create secret docker-registry regcred \
    --docker-server=private-registry.io \
    --docker-username=registry-user \
    --docker-password=registry-password \
    --docker-email=registry-user@org.com
```

```yaml
# nginx-pod.yaml
apiVersion: v1
kind: Pod
metadata:
  name: nginx-pod
spec:
  containers:
    - name: nginx
      image: private-registry.io/apps/internal-app
  imagePullSecrets:
    - name: regcred

# kubectl create -f nginx-pod.yaml
```
#### Security Contexts
Security contexts give the possibility to apply extra security measure at both POD or Container level, like specifying the id of the user in a POD the capabilities of this user,...

```yaml
# ubuntu-pod.yaml
apiVersion: v1
kind: Pod
metadata:
  name: nginx-pod
spec:
  containers:
    - name: ubuntu
      image: ubuntu
      command: ["sleep", "3600"]
      securityContext:
        runAsUser: 1000
        capabilities: ["MAC_ADMIN"]

# kubectl create -f ubuntu-pod.yaml
```
#### Network Policies
There are two types of traffic in a kubernetes Cluster :
- `Ingress :`Traffic entering a pod
- `Egress :` Traffic going out 

```yaml
# policy-definition.yaml
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: db-policy
spec:
  podSelector:
    matchLabels:
      role: db
  policyTypes:
    - Ingress
  ingress:
    - from:
        - podSelector:
            matchLabels:
              name: api-prod
        ports:
          - protocol: TCP
            port: 3306
```


## Container Orchestration - Networking
#### Cluster Networking
In a kubernetes Cluster ports associated to services are designed as follows: <br>
| Service | Port |
|-----|-------|
| `kube-api`| 6443
| `kubelet` | 10250
| `kube-scheduler` | 10259
| ` ETCD` | 2379
| ` kube-controller-manager` | 10257 |
| `services` | 30000 - 32767

#### POD Networking
#### Ingress Controller
```yaml
apiVersion: extension/v1beta1
kind: Deployment
metadata:
  name: nginx-ingress-controller
spec:
  replicas: 1
  selector:
    matchLabels:
      name: nginx-ingress
  template:
    metadata:
      labels:
        name: nginx-ingress
    spec:
      containers:
        - name: nginx-ingress-controller
          image: quay.io
```
## Container Orchestration - Service Mesh
#### Sidecars
Containers are encapsulated in pods and each pod can have one or more containers called sidecars. They share thesame network, memory and cpu resources with the main container in the POD

```yaml
# pod-definition.yaml
apiVersion: v1
kind: Pod
metadata:
  name: nginx-sidecar
spec:
  containers:
    - name: nginx-container
      image: nginx
      volumeMounts:
        - name: shared-data
          mountPath: /usr/share/nginx/html
    - name: sidecar-container
      image: fluent/fluentd
      volumeMounts:
        - name: shared-data
          mountPath: /pod-data
```
#### Envoy
Envoy is an open source proxy designed for distributed and microservices architecture. It is a proxy and communication Bus designed for data share between services and systems.

#### Service Mesh
A service mesh is a dedicated and configurable infrastructure layer that handles the communication between services without having to change the code in the microservice architecture. <br>

With a Service Mesh you can dynamically configure how services talk to each other with a mutual security configuration

#### Istio
It is a free and opensource service mesh that provides and efficient way to secure, connect and monitor services thereby bringing universal traffic management, telemetry and security to complex deployments. Istio works with kubernetes. 

#### Install Istio in a cluster
- we will use istioctl `istioctl install --set profile=demo -y`. There are many other profile depending on the environment where we want to deploy `[ prod | stage | dev | demo ]`
- istio is then installed as a deployemnt called `istiod` in a new namespace called `istio-system`
- it also deploy two services called: `istio-ingressgateway` and `istio-egressgateway`
- after installation verify the setup with the command `istio verify-install`

#### Deployonmg Our First Applicatoin on Istio
- `istioctl analyze`
- `kubectl label namespace default istio-injection=enabled`

## Container Orchestration - Storage
### Storage in Docker
#### File System
- when docker is installed in a system it creates a folder structure at `/var/lib/docker`

- `docker volume create data_volume` :  creates a folder in the folder `/var/lib/docker/volumes/data_volume`. This folder is termed `Persistent Volume`.
- `docker run -v data_volume:/var/lib/mysql mysql` : Run a MySQL database container with a volume mounted for data storage.

- There are two types of mounting in docker :
    - Volume Mount `docker run -v data_volume:/var/lib/mysql` => Mounting a Volume from the Docker Host
    - Bind  Mount `docker run -v /data/mysql:/var/lib/mysql`  => Mounting  a folder from the Docker Host

- `docker run --mount type=bind,source=data/mysql:/var/lib/mysqlmysql`.

### Volume Driver Plugins In Docker
- Storage Drivers help manage storage on containers : [AUFS|ZFS|BTRFS|DEVICE MAPPER|OVERLAY]
```yaml
# pod-definition.yaml
apiVersion: v1
kind: Pod
metadata:
  name: random-number-generator
spec:
  containers:
    - image: alpine
      name: alpine
      command: ["/bin/sh", "-c"]
      args: ["shuf -i 0-100 -n 1 >> /opt/number.out;"]
      volumeMounts:
      - mountPath: /opt
        name: data-volume

  volumes:
    - name: data-volume
      hostPath:
        path: /data
        type: Directory
    - name: aws-data-volume
      awsElasticBlockStore:
        volumeID: <volume-id>
        fsType: ext4
```
### Persistent Volumes in Kubernetes
A persitent volume is a cluster wide pool of storage volumes configured by an administrator
```yaml
# pv-definition.yaml
apiVersion: v1
kind: PersistentVolume
metatdata:
  name: pv-vol1
spec:
  accessModes:
    - ReadWriteOnce
  capacity:
    storage: 1Gi
  awsElasticBlockStore:
    volumeID: <volume-id>
    fsType: ext4
# kubectl create -f pv-definition.yaml
# kubectl get persistentvolume
```

### Persitent Volume Claims
```yaml
# pvc-definition.yaml
apiVersion: v1
kind: PersistentVolumeClaim
metatdata:
  name: myclaim
spec:
  accessModes:
    - ReadWriteOnce
  resources:
    requests:
      storage: 500Mi
  
# kubectl create -f pvc-definition.yaml
# kubectl get persistentvolumeclaim
```

### Storage Class
A Storage Class is a way of describing the clases of storage offered by admins.

```yaml
# sc-definition.yaml
apiVersion: storage.k8s.io/v1
kind: StorageClass
metadata:
  name: google-storage
provisioner: kubernetes.io/gce-pd
```
```yaml
# pvc-definition.yaml
apiVersion: v1
kind: PersistentVolumeClaim
metatdata:
  name: myclaim
spec:
  accessModes:
    - ReadWriteOnce
  storageClassName: google-storage
  resources:
    requests:
      storage: 500Mi
  
# kubectl create -f pvc-definition.yaml
# kubectl get persistentvolumeclaim
```

```yaml
# pod-definition.yaml
apiVersion: v1
kind: Pod
metadata:
  name: random-number-generator
spec:
  containers:
    - image: alpine
      name: alpine
      command: ["/bin/sh", "-c"]
      args: ["shuf -i 0-100 -n 1 >> /opt/number.out;"]
      volumeMounts:
      - mountPath: /opt
        name: data-volume

  volumes:
    - name: data-volume
      persistenVolumeClaim:
        claimName: myclaim
```

## Cloud Native Architecture
#### Horizontal Pod AutoScaler
```yaml
# deployment.yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: myapp
spec:
  replicas: 3
  selector:
    matchLabels:
      app: myapp
  template:
    metadata:
      labels:
        app: myapp
    spec:
      containers:
        - name: myapp
          image: myapp:latest
          resources:
            limits:
              cpu: 500m
            requests:
              cpu: 200m
```


```yaml
# hpa.yaml
apiversion: autoscaling/v2beta2
kind: HorizontalPodAutoscaler
metadata:
  name: myapp-hpa
spec:
  scaleTargetRef:
    apiVersion: apps/v1
    kind: Deployment
    name: myapp
  minReplicas: 1
  maxReplicas: 10
  metrics:
    - type: Resource
      resource:
        name: cpu
        target:
          type: Utilization
          averageUtilization: 50 
# kubectl create -f hpa.yaml
# kubectl get hpa
# kubectl delete hpa
```

#### Vertical Pod Autoscaler
- VPA recommender hits the metrics server to check and recommend the optimal resources for the pod then VPA Updater checks if the recommended resources match the current state of the Pod then the VPA Admission Controller takes action to update the new resources.
- VPA is not availbale on the cluster by default, yOu need to install and enable it before you can use it.

#### Cluster Autoscaler
The cluster autoscaler scales up the cluster by adding extra nodes when there is lack of resources for Pod scheduling on the existing nondes.

#### Kubernetes Enhancement Proposals (KEP's)

## Cloud Native Observability

## Cloud Native Application Delivery
