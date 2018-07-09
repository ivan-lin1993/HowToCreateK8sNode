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


### Trouble shooting
```
[WARNING FileExisting-ebtables]: ebtables not found in system path
[WARNING FileExisting-socat]: socat not found in system path

sudo apt install ebtables ethtool
sudo apt install socat
sudo apt install crictl
```