#### Container Storage Interface (CSI)
It is a specification that defines a standard for exposing ***block*** and ***file storage systems*** to containerized workloads on Container Orchestration Systems (COS) like Kubernetes. Its offers several benefits and addresses various challenges related to storage management in containerized environments.

Features
- Integrate different storage system with K8s
- Supports to multiple storage options
- Dynamic storage provisioning
- Snapshot & cloning
- Seamless migration
A volume can support local storage, on-premises software-defined storage, cloud-based storage (such as blob, block, or file storage), or a network file system (NFS).

Types of volume in K8s
1. Ephemeral volume
   Its targeted to the application need to hold the data, but they don’t care about data loss in the case that the pod fails or restarts – the lifecycle of the ephemeral volume is aligned with the pod lifecycle.
2. Persistent volume

