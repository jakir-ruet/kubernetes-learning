#### One master & two worker nodes configuration

##### Prerequisite
- You must know basic Linux command
- Basic command & understanding of docker & kubernetes.

###### Run these commands on each machine

```bash
apt-get update/upgrade
apt-get install apt-transport-https
```

###### Docker Install on each machine

- How to [Install?](https://docs.docker.com/get-docker/). You may install as per your operating system.
- Run the following command to uninstall all conflicting packages:

```bash
  for pkg in docker.io docker-doc docker-compose docker-compose-v2 podman-docker containerd runc; do sudo apt-get remove $pkg; done
```
- Repository setup
```bash
# Add Docker's official GPG key:
sudo apt-get update
sudo apt-get install ca-certificates curl
sudo install -m 0755 -d /etc/apt/keyrings
sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc
sudo chmod a+r /etc/apt/keyrings/docker.asc
```
```bash
# Add the repository to Apt sources:
echo \
   "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/ubuntu \
   $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \
   sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
sudo apt-get update
```
- Install

```bash
sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
docker --version
systemctl start docker
systemctl enable docker
```

###### setup open GPG key & configuration
```bash
sudo curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | sudo apt-key add
nano /etc/apt/sources.list.d/kubernetes.list
# put below line above/top of the 'kubernetes.list' file
deb http://apt.kubernetes.io/ kubernetes-xenial main
```

###### kubernetes packages install
```bash
apt-get update
apt-get install kubelet kubeadm kubectl kubernetes-cni -y
```

###### 'kubeadm' initialize a Kubernetes on control-plane node
```bash
kubeadm init
# after initialize, we will see below script
mkdir -p $HOME/.kube
cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
sudo chown $(id -u):(id -g) $HOME/.kube/config
# or
export KUBERCONFIG=/etc/kubernetes/admin.conf
```

###### [Flanner](https://github.com/flannel-io)
Flannel is a popular networking solution for Kubernetes clusters. It is designed to provide a simple and reliable way to set up an overlay network that connects pods across different nodes in a Kubernetes cluster.
```bash
kubectl apply -f https://raw.githubusercontent.com/coreos/flannel/master/Documentation/kube-flannel.yml
```

###### worker node configure (copy paste on worker node)
```bash
# it will be generated after 'kubeadm init'
kubeadm join 172.31.14.32:6443 --token mcvd2n.aqvyp59vq3inhtks --discovery-token-ca-cert-hash sha256:5ab6e6d695d31c05a4e4cde8f6e7b14ff0d0b450383e9b255270be90904de128
```

###### Check the node from master machine, whether its working or not
```bash
kubectl get nodes
```

###### port & service allow on firewall

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