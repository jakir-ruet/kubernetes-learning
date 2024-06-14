[Networking](https://kubernetes.io/docs/concepts/cluster-administration/networking/)
is a central part of Kubernetes, but it can be challenging to understand exactly how it is expected to work. There are 4 distinct networking problems to address:

1. Highly-coupled container-to-container communications: this is solved by Pods and localhost communications.
2. Pod-to-Pod communications: this is the primary focus of this document.
3. Pod-to-Service communications: this is covered by Services.
4. External-to-Service communications: this is also covered by Services.

***K8s Network Model***
- use calico network model
- calico & kubeadm the best pair
- node communicate with each pod using NAT
- pod communicate each other using IP/DNS Record
- every pod get its own IP 

***Container Network Interface (CNI):***
- As an essential component of the Kubernetes ecosystem, CNI enables seamless communication and connectivity between containers and external networks.
- It serves as a bridge between the container runtime and the network plugins, allowing for the dynamic configuration of networking for Kubernetes pods. CNI plugins handle tasks such as 
  - assigning IP addresses, 
  - creating network interfaces, and 
  - setting up network routes for containers.

***How it works***
 - after crating a pod in k8s the container runtime (docker) call the CNI plugins to set the network environment.
 - it can leverage the Linux networking stack to configure networking for containers.

***DNS in K8s***
Kubernetes creates DNS records (1. Services and 2. Pods). You can contact Services with consistent DNS names instead of IP addresses.
- DNS run as a service in 'kube-system' namespace
- kubeadm/minikube use Core DNS
- All pods in kubeadm cluster are automatically given domain name like `pod-id.namespace-name.pod.cluster.local`
- Pod DNS in default namespace with IP 192.168.1.120 then it would be `192-168-1-12.default.pod.cluster.local`

```bash
kubectl get pods -o wide -n kube-system
kubectl get services -o wide -n kube-system
kubectl apply -f pod-dns.yaml
kubectl get pods -o wide
# execute fro host machine
curl 10-244-0-58.default.pod.cluster.local # getting an error
kubectl exec -it frontend-app sh
apk update
apk install curl
curl --version
# execute fro host machine
curl 10-244-0-58.default.pod.cluster.local # we will see nginx default page
```

***Network Policy*** is an object of K8s & its control the traffic flow at IP address or port level.
Pods can communicate using ***three*** Identifiers such as
- Other pods that are allowed
- namespaces that are allowed
- IP blocks/range
Network policies allows to build a secure network by keeping pod isolated from traffic they do not need. By default pods are non isolated, they accept traffic from any source. pods become isolated by having a network policy that selects them. the network policy components are 
- pod selector
  - determine to which pods in namespace the network policy will be applied
  - can select the pods using labels
  - an empty podSelector selects all pods in the namespace
  ```bash
  ...
  spec:
    podSelector:
      matchLabels:
        role: frontend-app
  ...
  ```
- ingress-egress (inbound-outbound traffic network policy apply)
  - formSelector: selects Ingress traffic that will be allowed on pods
  - toSelector: selects Egress traffic that will be allowed from pods
- fromSelector & toSelector
  PodSelector
  ```bash
  ...
  ingress:
    - from:
      - podSelector:
          matchLabels:
            role: client
  ...
  ```
  Namespace Selector
  ```bash
  ...
  ingress:
    - from:
      - namespaceSelector:
          matchLabels:
            role: client
  ...
  ```
  IP Block
  ```bash
  ...
  ingress:
    - from:
      - ipBlock:
          192.168.1.120/32 # range [192.168.1.0-192.168.1.255]
  ...
  ```
- Ports (Specify one/more ports that allow traffic)
 Ports
  ```bash
  ...
  ingress:
    - from:
      - ports:
          - protocol: TCP
            port: 80 # could be multiple pods
  ...
  ```
    ```bash
  ...
  egress:
    - from:
      - ports:
          - protocol: TCP
            port: 2020
            endPort: 5050
  ...
  ``` 

