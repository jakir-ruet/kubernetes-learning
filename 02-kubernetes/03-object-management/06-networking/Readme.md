[Networking](https://kubernetes.io/docs/concepts/cluster-administration/networking/)
Networking is a central part of Kubernetes, but it can be challenging to understand exactly how it is expected to work. There are 4 distinct networking problems to address:

1. Highly-coupled container-to-container communications: this is solved by Pods and localhost communications.
2. Pod-to-Pod communications: this is the primary focus of this document.
3. Pod-to-Service communications: this is covered by Services.
4. External-to-Service communications: this is also covered by Services.

K8s Network Model
- use calico network model
- calico & kubeadm the best pair
- node communicate with each pod using NAT
- pod communicate each other using IP/DNS Record
- every pod get its own IP 

Calico network plugin
- 