# QUESTION 9 : Backup and restore ETCD Server

- Install etcdctl utility client 

```bash
sudo apt update

sudo apt instal etcd-client -y
```

- Set the environment variables requirement for the etcdctl utility tools to work best
```bash

cat >> /etc/environment << EOF
ENDPOINT=https://127.0.0.1:2379
ETCDCTL_API=3
ETCDCTL_CACERT=/etc/kubernetes/pki/etcd/ca.crt
ETCDCTL_CERT=/etc/kubernetes/pki/etcd/server.crt
ETCDCTL_KEY=/etc/kubernetes/pki/etcd/server.key
EOF
```

```bash
export ETCDCTL_API=3

etcdctl --endpoints=https://127.0.0.1:2379 \
    --cacert=/etc/kubernetes/pki/etcd/ca.crt \
    --cert=/etc/kubernetes/pki/etcd/server.crt \
    --key=/etc/kubernetes/pki/etcd/server.key \
    snapshot save /opt/etcd-backup.db

```