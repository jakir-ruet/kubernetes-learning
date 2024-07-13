#### Welcome to K8s Securing
Kubernetes security is a broad topic due to the sophistication of the platform. It includes secure Kubernetes **nodes**, **networks**, and Kubernetes objects such as **Pods**. The Cloud Native Computing Foundation (CNCF) defines Kubernetes security in layers, which they call the four Cs of cloud-native security, taking the topic of security beyond Kubernetes and its ecosystem. The **four C**s stand for **Cloud**, **Cluster**, **Container**, and **Code**, as shown in the following diagram:
![K8s Security Layer](/img/security/security-layer.png)
- Cloud
  it is managed by the cloud provider when it is in the cloud or by the organization when it comes to a private data center.
- Cluster
  It is more about securing the Kubernetes cluster components, ensuring each component is secured and conjured correctly. 
- Container
  Its includes container vulnerability scanning, hosted OS scaling, and container privileged users.
- Code
  It is focused on the application code. Different from traditional application security approaches, it now works with DevSecOps and vulnerability assessment tools. 

**Service accounts versus user accounts**
In Kubernetes, we have a distinction between normal user accounts and service accounts managed by Kubernetes. An account represents an identity for a user or a service process. The main difference between a user account and a service account is as follows:

1. User accounts are for normal human users. In Kubernetes, the RBAC subsystem is used to determine whether the user is authorized to perform a specific operation on a specific scope. We’ll look into this further in the Kubernetes RBAC section as I discussion previous.
2. **Service accounts** are for services or processes running in a Pod in the Kubernetes cluster. The service accounts are users managed by the Kubernetes API. In Kubernetes, it is possible to use client certificates, bearer tokens, or even an authenticating proxy to authenticate API requests through an API server.

**K8s authorization**
