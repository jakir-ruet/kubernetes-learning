#### One master & two worker nodes configuration

##### Prerequisite
- You must know basic Linux command
- Basic command & understanding of docker & kubernetes.

###### upgrade the & put the host name of all node.
```bash
sudo apt-get update
sudo apt-get upgrade
nano /etc/hosts # setup all host
192.168.3.37 k8s-control-node
192.168.3.38 k8s-workerone-node
192.168.3.39 k8s-workertwo-node
```
```bash
cat <<EOF | sudo tee /etc/modules-load.d/containerd.conf
```
```bash
overlay
```
```bash
br_netfilter
```
```bash
EOF
```
```bash
sudo modprobe overlay
sudo modprobe br_netfilter
```

```bash
cat <<EOF | sudo tee /etc/sysctl.d/99-kubernetes-cri.conf
```
```bash
net.bridge.bridge-nf-call-iptables=1
```
```bash
net.ipv4.ip_forward=1
```
```bash
net.bridge.bridge-nf-call-ip6tables=1
```
```bash
EOF
```

```bash
sudo apt-get update
sudo apt-get install curl
sudo apt-get install ca-certificates
sudo apt-get install gnupg
``` 

```bash
sudo apt-get update && sudo apt-get install -y containerd.io
sudo mkdir -p /etc/containerd
sudo containerd config default | sudo tee /etc/containerd/config.toml

```
```bash
sudo swapoff -a
sudo systemctl restart containerd
sudo systemctl status containerd
curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | sudo apt-key add -
```
```bash
sudo <<EOF tee | sudo tee /etc/apt/source.list.d/kubernetes.list
```
```bash
deb https://apt.kubernetes.io/kubernetes-xenial main
```
```bash
EOF
```
```bash
sudo apt-get update
sudo apt-get install kubelet kubeadm kubectl # or
sudo apt-get install kubelet=1.30.0-00 kubeadm=1.30.0-00 kubectl=1.30.0-00
sudo sysctl --system # reload
```

```bash
sudo apt-mark hold kubectl kubeadm kubelet # only for control plane
sudo kubeadm init # or
sudo kubeadm init --pod-network-cidr 192.168.1.36/24 --Kubernetes version: v1.30.0
# here 192.168.1.36 is cluster IP
sudo sysctl --system # reload
kubectl get all
```

```bash
# after initialize, we will see below script
mkdir -p $HOME/.kube
sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
sudo chown $(id -u):$(id -g) $HOME/.kube/config
kubectl get nodes
```

Install network plugin
```bash
kubectl apply -f cat <<EOF | sudo tee /etc/sysctl.d/99-kubernetes-cri.conf
```
```bash
net.bridge.bridge-nf-call-iptables=1
```
```bash
net.ipv4.ip_forward=1
```
```bash
net.bridge.bridge-nf-call-ip6tables=1
```
```bash
EOF
```

See [Networking and Network Policy](https://kubernetes.io/docs/concepts/cluster-administration/addons/)

[Install Calico](https://docs.tigera.io/calico/latest/getting-started/kubernetes/quickstart)
```bash
kubectl apply —f https://raw.githubusercontent.com/projectcalico/calico/y3.25.1/manifests/calico.yaml 
```

Run the token on working node
```bash
kubectl get nodes
kubectl get all
kubeadm token create --print-join-command
sudo sysctl --system # reload
```
Run this token on all worker nodes
```bash
kubeadm join 192.168.3.37:6443 --token n87ioa.vl1kn5wpdjworycb \
        --discovery-token-ca-cert-hash sha256:11930fc6ee2310b16788321ae62301a9bebb52b6438eef5011b54b0dd6922482
sudo sysctl --system # reload
```

On control plane
```bash
kubectl get nodes

###### [setup open GPG key & configuration](https://kubernetes.io/docs/tasks/tools/install-kubectl-linux/#install-using-native-package-management)
```bash
sudo apt-get update
# Update the apt package index and install packages needed to use the Kubernetes apt repository:
sudo apt-get install -y apt-transport-https ca-certificates curl
# Download the public signing key for the Kubernetes package repositories.
curl -fsSL https://pkgs.k8s.io/core:/stable:/v1.30/deb/Release.key | sudo gpg --dearmor -o /etc/apt/keyrings/kubernetes-apt-keyring.gpg
# allow unprivileged APT programs to read this keyring
sudo chmod 644 /etc/apt/keyrings/kubernetes-apt-keyring.gpg
# This overwrites any existing configuration in /etc/apt/sources
echo 'deb [signed-by=/etc/apt/keyrings/kubernetes-apt-keyring.gpg] https://pkgs.k8s.io/core:/stable:/v1.30/deb/ /' | sudo tee /etc/apt/sources.list.d/kubernetes.list
# helps tools such as command-not-found to work correctly
sudo chmod 644 /etc/apt/sources.list.d/kubernetes.list
```

###### [port & service allow on firewall](https://www.ibm.com/docs/en/cdfsp/7.6.1.x?topic=kubernetes-installing-kubeadm-kubelet-kubectl)

|  SL   | Protocol | Direction | Port-Range  | Purpose                 | Used By               |
| :---: | :------- | :-------- | :---------- | :---------------------- | :-------------------- |
|   1   | TCP      | Inbound   | 6443*       | Kubernetes API server   | All                   |
|   2   | TCP      | Inbound   | 2379- 2380  | etcd server client api  | kube-apiserver , etcd |
|   3   | TCP      | Inbound   | 10250       | Kubelet API             | Self control,plane    |
|   4   | TCP      | Inbound   | 10251       | Kube-Schedular          | Self                  |
|   5   | TCP      | Inbound   | 30000-32767 | Nodemon-services+       | All                   |
|   6   | TCP      | Inbound   | 10252       | Kube-controller manager | Self                  |
|   7   | TCP      | Inbound   | 22          | SSH                     | Self                  |
|   8   | TCP      | Inbound   | 80          | HTTP                    | Self                  |
|   9   | TCP      | Inbound   | 443         | HTTPS                   | Self                  |
|  10   | TCP      | Inbound   | 5432        | Postgres                | postgres              |