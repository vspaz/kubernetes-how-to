# kubernetes how-to
kubernetes how-to, set up &amp;&amp; configure


## set up kubernetes cluster on ubuntu 16.04+

### Master

switch to root
```bash
sudo su
```

turn off swap
```bash
swapoff -a

vim /etc/fstab  # comment out swap line
```

install docker

```bash
apt-get update && apt-get install -qy docker.io
```

```bash
apt-get update && apt-get install -y apt-transport-https && curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | apt-key add -
```

```bash
echo "deb http://apt.kubernetes.io/ kubernetes-xenial main" | tee -a /etc/apt/sources.list.d/kubernetes.list  && apt-get update
```

```bash
apt-get install -y kubelet kubeadm  kubernetes-cni
```

Get your master node ip

```bash
ifconfig
```

```bash
kubeadm init --pod-network-cidr=10.244.0.0/16 --apiserver-advertise-address=192.168.122.245 --kubernetes-version stable-1.14
```

Switch to regular user

```bash
su master
```

```bash
sudo cp /etc/kubernetes/admin.conf $HOME/
```

```bash
sudo chown $(id -u):$(id -g) $HOME/admin.conf
```

```bash
export KUBECONFIG=$HOME/admin.conf
```

```bash
echo "export KUBECONFIG=$HOME/admin.conf" | tee -a ~/.bashrc
```

```bash
export KUBECONFIG=$HOME/admin.conf
```

```bash
kubectl apply -f https://raw.githubusercontent.com/coreos/flannel/master/Documentation/kube-flannel.yml
```

```bash
kubectl apply -f https://raw.githubusercontent.com/coreos/flannel/master/Documentation/k8s-manifests/kube-flannel-rbac.yml
```

### Minion


switch to root
```bash
sudo su
```

turn off swap
```bash
swapoff -a

vim /etc/fstab  # comment out swap line
```

install docker

```bash
apt-get update && apt-get install -qy docker.io
```

```bash
apt-get update && apt-get install -y apt-transport-https && curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | apt-key add -
```

```bash
echo "deb http://apt.kubernetes.io/ kubernetes-xenial main" | tee -a /etc/apt/sources.list.d/kubernetes.list  && apt-get update
```

```bash
apt-get install -y kubelet kubeadm  kubernetes-cni
```

```bash
kubeadm join 192.168.122.245:6443 --token 2ek4u2.7o7dy2m4wflmb9ws --discovery-token-ca-cert-hash sha256:40f4db486b20265637b014e9d0ee762ef37302809aab425cd63da2b53569fb2d
```

```bash
kubectl get nodes -o wide
```

voilà!
