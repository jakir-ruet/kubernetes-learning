#### Welcome to master slave node configuration
##### Master node configuration
By follow these steps we can configure master node
- Step-1: (update & upgrade)
```bash
sudo apt-get update
sudo apt-get upgrade
sudo swapoff -a # Disable swap memory
```
- Step-2: [(Install Docker Engine/Container Runtime)](https://docs.docker.com/engine/install/)

Remove the old version
```bash
for pkg in docker.io docker-doc docker-compose podman-docker containerd runc; do sudo apt-get remove $pkg; done
```
```bash
sudo apt-get update
sudo apt-get install ca-certificates curl
sudo install -m 0755 -d /etc/apt/keyrings
sudo curl -fsSL https://download.docker.com/linux/debian/gpg -o /etc/apt/keyrings/docker.asc
sudo chmod a+r /etc/apt/keyrings/docker.asc
sudo apt-get update
```
```bash
sudo apt-get install -y docker.io
sudo systemctl restart containerd
sudo systemctl status containerd
```

```bash
# Configure containerd
cat <<EOF | sudo tee /etc/modules-load.d/containerd.conf
overlay
br_netfilter
sudo modprobe overlay
sudo modprobe br_netfilter
EOF
```
Step-3: [Configure Prerequisites Network configuration](https://kubernetes.io/docs/setup/production-environment/container-runtimes/)
```bash
# sysctl params required by setup, params persist across reboots
cat <<EOF | sudo tee /etc/sysctl.d/k8s.conf
net.bridge.bridge-nf-call-iptables=1
net.ipv4.ip_forward = 1
sysctl net.ipv4.ip_forward # Verify that net.ipv4.ip_forward is set to 1 with
net.bridge.bridge-nf-call-ip6tables=1
EOF
# Apply sysctl params without reboot
sudo sysctl --system
```

- Step-4: (Install Supported packages)
```bash
sudo apt-get install -y apt-transport-https curl
```
- Step-5: (Getting key for kubernetes repo add it to my key manager)
```bash
curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | sudo apt-key add -
```
- Step-6: (Add the kubernetes repo to your system)
   - [Installing kubeadm, kubelet and kubectl](https://kubernetes.io/docs/setup/production-environment/tools/kubeadm/install-kubeadm/)
   - [Install using native package management](https://kubernetes.io/docs/tasks/tools/install-kubectl-linux/#install-kubectl-binary-with-curl-on-linux)
```bash
pager /etc/apt/sources.list.d/kubernetes.list # check the list
sudo apt-get update
# install supported packages
sudo apt-get install -y apt-transport-https ca-certificates curl gnupg
# If the folder `/etc/apt/keyrings` does not exist, it should be created before the curl command, read the note below.
# sudo mkdir -p -m 755 /etc/apt/keyrings
curl -fsSL https://pkgs.k8s.io/core:/stable:/v1.30/deb/Release.key | sudo gpg --dearmor -o /etc/apt/keyrings/kubernetes-apt-keyring.gpg
sudo chmod 644 /etc/apt/keyrings/kubernetes-apt-keyring.gpg # allow unprivileged APT programs to read this keyring
# This overwrites any existing configuration in /etc/apt/sources.list.d/kubernetes.list
echo 'deb [signed-by=/etc/apt/keyrings/kubernetes-apt-keyring.gpg] https://pkgs.k8s.io/core:/stable:/v1.30/deb/ /' | sudo tee /etc/apt/sources.list.d/kubernetes.list
sudo chmod 644 /etc/apt/sources.list.d/kubernetes.list   # helps tools such as command-not-found to work correctly
```
Alternative
```bash
cat <<EOF >/etc/apt/sources.list.d/kubernetes.list
deb http://apt.kubernetes.io/ kubernetes-xenial main
EOF
```
- Step-7: (Install kubeadm, kubelet and kubectl)
[Ports and Protocols](https://kubernetes.io/docs/reference/networking/ports-and-protocols/)
```bash
nc 127.0.0.1 6443 -v # Check required ports, it not available allow on ufw.
sudo apt-get update
sudo apt-get install -y kubelet kubeadm kubectl
sudo apt-mark hold kubelet kubeadm kubectl # for prevent these specific Kubernetes packages
```

- Step-8: (Initialize a Kubernetes master node)
```bash
# [see](https://docs.tigera.io/calico/latest/getting-started/kubernetes/quickstart)
sudo kubeadm init # or
sudo kubeadm init --pod-network-cidr=192.168.0.0/16 --Kubernetes version: v1.30.0
# here 192.168.0.0 is cluster IP
sudo sysctl --system # reload
```
```bash
# it will be generated after executing 'sudo kubeadm init --pod-network-cidr=192.168.0.0/16'
mkdir -p $HOME/.kube
sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
sudo chown $(id -u):$(id -g) $HOME/.kube/config
# alternatively, if you are the root user, you can run:
export KUBECONFIG=/etc/kubernetes/admin.conf
# node connecting token generated
kubeadm join 192.168.106.129:6443 --token ae775e.oej3ou6yqhbqlcer \
	--discovery-token-ca-cert-hash sha256:d5c353b3b41c4f8cd206deb51785fa7b162f60470860958da33bfba39bd504d5 
```

- Step-9: (Install the Calico network plugin)
```bash
kubectl apply -f https://docs.projectcalico.org/manifests/calico.yaml
```
- Step-10: (Untaint the master so that it will be available for scheduling workloads)
```bash
kubectl taint nodes --all node-role.kubernetes.io/master-
```
- Step-11. (Check nodes)
```bash
kubectl get nodes
```
Step: 12 (For worker node)
```bash
kubectl get nodes
kubectl get all
kubeadm token create --print-join-command
sudo sysctl --system # reload
kubectl get nodes
kubectl get all
```