#### Welcome to maintaining-clusters
- What is High Availability?
- HA Control Plane
- Etch Management
  - Stacked Etcd
  - External Etcd
- K8s Management Tools
  - Kubectl
    - Kubectl is oﬃcial CLI for K8s.
    - We will see using kubectl thought-out this course.
  - Kubeadm
    - Kubeadm tool is used to easily creating the K8s Cluster.
    - Helping user to set-up and make cluster functional.
  - Minikube (Kind & others)
    - K8s Developers tool
    - Local single node cluster
  - Helm
    - Powerful tool for K8s Template & Package Manager
    - Use for complex multi config template.
  - Kompose
    - Translate Docker Compose files into a K8s Objects
    - Ship containers from compose to K8s
  - Kustomize
    - Configuration management tool for K8s objects configuration
    - Similar to Helm and have ability to create re-useable templates for K8s.

#### Lets get started Drain/Remove Node From Cluster
- Node Draining
- How to Drain a Node
- Ignore DaemonSets
- Uncordoning a Node

#### Lets get started Upgrade the cluster
- Master node upgrade
  - Drain node
  - Upgrade plan
  - Apply upgrade
  - Upgrade kubectl & kubelet
  - Uncordon node & Join
- Worker node upgrade
  - Drain node
  - Upgrade kubeadm
  - Upgrade kubelet config
  - Upgrade kubectl & kubelet
  - Uncordon node