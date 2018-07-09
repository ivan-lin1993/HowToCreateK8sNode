# How to create a K8S node on Ubuntu

## Install Utility
### Install Docker
```
sudo apt update
sudo apt install docker.io

## 免用 root
sudo gpasswd -a $USER docker
sudo reboot
```
### Install kubeadm, kubelet, kubectl

- kubeadm : 幫助啟動一個k8s cluster
- kubelet : cluster 中每個機器的運行配置
- kubectl : 對 cluster 下指令的工具

```
apt-get update && apt-get install -y apt-transport-https curl
curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | apt-key add -
cat <<EOF >/etc/apt/sources.list.d/kubernetes.list
deb http://apt.kubernetes.io/ kubernetes-xenial main
EOF
apt-get update
apt-get install -y kubelet kubeadm kubectl

```

### 把swap禁掉
如果出現
```
[ERROR Swap]: running with swap on is not supported. Please disable swap
[preflight] If you know what you are doing, you can make a check non-fatal with `--ignore-preflight-errors=...`
```
disable swap
```
sudo swapoff -a
```

## Try kubeadm
### Create Master a node
```
sudo kubeadm init
```
### 設定kubectl
```
mkdir -p $HOME/.kube
sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
sudo chown $(id -u):$(id -g) $HOME/.kube/config
```
### 試試看kubectl
```
kubectl get nodes
```

You must install a pod network add-on so that your pods can communicate with each other.

The network must be deployed before any applications. Also, CoreDNS will not start up before a network is installed. kubeadm only supports Container Network Interface (CNI) based networks (and does not support kubenet).
```
kubectl apply -f https://docs.projectcalico.org/v3.1/getting-started/kubernetes/installation/hosted/rbac-kdd.yaml
kubectl apply -f https://docs.projectcalico.org/v3.1/getting-started/kubernetes/installation/hosted/kubernetes-datastore/calico-networking/1.7/calico.yaml
```

### Trouble shooting
```
[WARNING FileExisting-ebtables]: ebtables not found in system path
[WARNING FileExisting-socat]: socat not found in system path

sudo apt install ebtables ethtool
sudo apt install socat
sudo apt install crictl
```