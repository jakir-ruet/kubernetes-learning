#### One master & two worker nodes configuration

##### Prerequisite
- You must know basic Linux command
- Basic command & understanding of docker & kubernetes.

###### Upgrade VM/Machine
```bash
sudo apt-get update
sudo apt-get upgrade
```

Setup IP addresses in all VM/Machine
```bash
nano /etc/hosts # setup all host
192.168.3.37 k8s-control-node
192.168.3.38 k8s-workerone-node
192.168.3.39 k8s-workertwo-node
```

Configure containerd
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

Configure 99-kubernetes-cri
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

Install these dependencies
```bash
sudo apt-get update
sudo apt-get install curl
sudo apt-get install ca-certificates
sudo apt-get install gnupg
``` 

###### Docker Installation
How to [Install?](https://docs.docker.com/get-docker/). You may install as per your operating system.
Run the following command to uninstall all conflicting packages:
```bash
for pkg in docker.io docker-doc docker-compose docker-compose-v2 podman-docker containerd runc; do sudo apt-get remove $pkg; done
```
Repository setup
```bash
# Add Docker's official GPG key:
sudo apt-get update
sudo apt-get install ca-certificates curl
sudo install -m 0755 -d /etc/apt/keyrings
sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc
sudo chmod a+r /etc/apt/keyrings/docker.asc
```

```bash
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/ubuntu \
  $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
sudo apt-get update
sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```

Checking configuration
```bash
sudo mkdir -p /etc/containerd
sudo containerd config default | sudo tee /etc/containerd/config.toml
```

Disable swap memory & add apt-key
```bash
sudo swapoff -a
sudo systemctl restart containerd
sudo systemctl status containerd
curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | sudo apt-key add -
```

###### [Setup open GPG key & Configuration](https://kubernetes.io/docs/tasks/tools/install-kubectl-linux/#install-using-native-package-management)
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

[Installing kubeadm, kubelet and kubectl](https://v1-29.docs.kubernetes.io/docs/setup/production-environment/tools/kubeadm/install-kubeadm/)
```bash
sudo apt-get update
sudo apt-get install -y apt-transport-https ca-certificates curl gpg
curl -fsSL https://pkgs.k8s.io/core:/stable:/v1.30/deb/Release.key | sudo gpg --dearmor -o /etc/apt/keyrings/kubernetes-apt-keyring.gpg
echo 'deb [signed-by=/etc/apt/keyrings/kubernetes-apt-keyring.gpg] https://pkgs.k8s.io/core:/stable:/v1.30/deb/ /' | sudo tee /etc/apt/sources.list.d/kubernetes.list
sudo apt-get update
sudo apt-get install -y kubelet kubeadm kubectl
sudo apt-mark hold kubelet kubeadm kubectl # only control plane
sudo sysctl --system # reload
```
Disable swap memory & kubeadm initialize
```bash
sudo swapoff -a
sudo kubeadm init # or
sudo kubeadm init --pod-network-cidr=192.168.0.0/16 --Kubernetes version: v1.30.0 # [see](https://docs.tigera.io/calico/latest/getting-started/kubernetes/quickstart)
# here 192.168.0.0 is cluster IP
sudo sysctl --system # reload
```

Give admin permission
```bash
# after initialize, we will see below script
mkdir -p $HOME/.kube
sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
sudo chown $(id -u):$(id -g) $HOME/.kube/config
# or, if you are the root user, you can run
export KUBECONFIG=/etc/kubernetes/admin.conf
kubectl get nodes
```

You should now deploy a pod network to the cluster.
Run "kubectl apply -f [podnetwork].yaml" with one of the options listed to this [link](https://kubernetes.io/docs/concepts/cluster-administration/addons/).

Install Calico
```bash
kubectl create -f https://raw.githubusercontent.com/projectcalico/calico/v3.27.3/manifests/tigera-operator.yaml
```

Then you can join any number of ***worker nodes*** by running the following on each as root:
```bash
kubeadm join 172.16.246.135:6443 --token 8icerf.nk1x7f11ptx7uroi \
	--discovery-token-ca-cert-hash sha256:8fcded6e57287119388da689e6e7d2959e4e2336cdac7f846aab53209915df27 
```

You may create token for connecting worker node
```bash
kubectl get nodes
kubectl get all
kubeadm token create --print-join-command
sudo sysctl --system # reload
```

###### [Port & Service allow on Firewall](https://www.ibm.com/docs/en/cdfsp/7.6.1.x?topic=kubernetes-installing-kubeadm-kubelet-kubectl)

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

###### Everything Done, Have a Good Day