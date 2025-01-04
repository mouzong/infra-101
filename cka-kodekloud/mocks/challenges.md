# QUESTION 9 : Backup and restore ETCD Server

- Install etcdctl utility client 

```bash
sudo apt update

sudo apt install etcd-client -y

etcdctl snapshot
```

- Set the environment variables requirement for the etcdctl utility tools to work best

```bash
cat >> ~/.vimrc << EOF
syntax on
set ts=2 sts=2 sw=2 et nu ai cuc cul termguicolors
colorscheme dracula
filetype plugin indent on
EOF

source ~/.vimrc

cat >> ~/.bashrc << EOF
export ENDPOINT=https://127.0.0.1:2379
export ETCDCTL_API=3
export ETCDCTL_CACERT=/etc/kubernetes/pki/etcd/ca.crt
export ETCDCTL_CERT=/etc/kubernetes/pki/etcd/server.crt
export ETCDCTL_KEY=/etc/kubernetes/pki/etcd/server.key
EOF

source ~/.bashrc
```

```bash
etcdctl --endpoints=https://127.0.0.1:2379 \
    --cacert=/etc/kubernetes/pki/etcd/ca.crt \
    --cert=/etc/kubernetes/pki/etcd/server.crt \
    --key=/etc/kubernetes/pki/etcd/server.key \
    snapshot save /opt/etcd-backup.db

etcdctl --endpoints=$ENDPOINT snapshot save /opt/etcd-backup.db

# verify the status of the snapshot
etcdctl --write-out=table snapshot status /opt/etcd-backup.db

```

## Restore ETCD

```bash
# Stop all api-servers and restart kube-scheduler, kube-controller-manager, kubelet instances 

# stopping all 
sudo mv /etc/kubernetes/manifests/*.yaml /tmp

sudo systemctl daemon-reload

sudo systemctl restart kubelet
```

```bash
etcdctl snapshot restore  /opt/etcd-backup.db --endpoints=$ENDPOINT --data-dir=/var/lib/etcd-restore-from-backup
```

```bash
# edit the --data-dir location and volume mount paths
vim /tmp/etcd/yaml

```


### Question 4

Autoscale a deployment with horizontal Pod Atoscaler 

```bash
kubectl -n production autoscale deploy frontend --min=3 --max=5 --cpu-percent=80
```
