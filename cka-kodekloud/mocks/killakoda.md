## Question 1 : Join node01 to luster

```bash
kubeadm token create --print-join-command

ssh node01

kubeadm join 172.30.1.2:6443 --token ...

exit

kubectl get node
```

## Question 2 : Upgrade cluster

- On master node

```bash
# see possible versions
kubeadm upgrade plan

# show available versions
apt-cache show kubeadm

# can be different for you
apt-get install kubeadm=1.31.1-1.1

# could be a different version for you, it can also take a bit to finish!
kubeadm upgrade apply v1.31.1

# can be a different version for you
apt-get install kubectl=1.31.1-1.1 kubelet=1.31.1-1.1

service kubelet restart
```

## Question 3 : Using kubeadm, read out the expiration date of the apiserver certificate and write it into /root/apiserver-expiration .

```bash
kubeadm certs check-expiration | grep apiserver

echo 'Jan 02, 2026 09:47 UTC' > /root/apiserver-expiration

# Renew certifictaes
kubeadm certs renew apiserver

kubeadm certs renew scheduler.conf

kubeadm certs check-expiration 
```

## Question 3 : Vim setup for CKA
```bash
vim ~/.vimrc

set expandtab
set tabstop=2
set shiftwidth=2
```