##### [Volumes](https://kubernetes.io/docs/concepts/storage/)
It is a directory containing data, which can be accessed by containers in a Kubernetes pod. The location of the directory, the storage media that supports it, and its contents, depend on the specific type of volume being used. There are a few types of volumes in Kubernetes.

- Volumes
  - Persistent Volumes (PV)
    is a piece of storage in the cluster that has been provisioned by an administrator or dynamically provisioned using `Storage Classes`. It is a resource in the cluster just like a node is a cluster resource. PVs are volume plugins like Volumes, but have a lifecycle independent of any individual Pod that uses the PV.
  - Persistent Volume Claim (PVC)
    - It is a request for storage by a user. 
    - It is similar to a Pod. 
    - Pods consume node resources and 
    - PVCs consume PV resources.
    Pods can request specific levels of resources (CPU and Memory). Claims can request specific `size` and `access` modes (They can be mounted to access mode) 
      - ReadWriteOnce, 
      - ReadOnlyMany, 
      - ReadWriteMany, or 
      - ReadWriteOncePod
  - Ephemeral Volumes
  - EmptyDir Volumes
  - hostPath Volumes
  - Volumes ConfigMap 
- [Storing Volumes](https://kubernetes.io/docs/concepts/storage/storage-classes/) 
  - NFS (Network File System)
  - CSI (Container Storage Interface)

**StorageClass**
A StorageClass in Kubernetes is a way to define different types of storage, or "classes," that a cluster administrator offers. It provides a way for cluster administrators to describe the "classes" of storage they offer and allows users to request different types of storage dynamically based on their performance and cost requirements.

**Features of StorageClass**
- **Dynamic Provisioning:** A StorageClass enables dynamic provisioning of Persistent Volumes (PVs). When a user creates a Persistent Volume Claim (PVC) that references a StorageClass, Kubernetes automatically provisions a Persistent Volume that matches the desired storage properties defined in the StorageClass.
- **Abstracts Underlying Storage:** It abstracts the details of the underlying storage infrastructure (such as type, performance, availability zone, etc.). Users only need to specify the required class of storage (e.g., fast, slow, ssd) without needing to know the specifics of how it is implemented.
- **Supports Different Backends:** StorageClasses can be configured to support various storage backends such as AWS EBS, Google Cloud Persistent Disks, Azure Disks, NFS, Ceph, GlusterFS, and more. This flexibility allows Kubernetes to work with different types of storage solutions.
- **Customizable Parameters:** Each StorageClass can define a set of parameters that affect how the storage is provisioned. These parameters are specific to the storage backend and can include details such as disk type, IOPS, redundancy level, and more.
- **Reclaim Policy:** StorageClasses define a reclaim policy that dictates what happens to a dynamically provisioned Persistent Volume when it is released (e.g., deleted). The reclaim policy can be Retain (keep the storage intact), Delete (delete the storage), or Recycle (wipe and reuse the storage).

***Volume Types***
- Volume & Persistent Volumes each have a Volume Type.
- Volume Type determines how storage will be handled.
- Various Volume Types supports in K8s:
  - NFS - Network file System
  - Cloud Storage - AWS, GCP, Azure
  - ConfigMaps & Secrets
  - File System on K8s Node

***Volumes & Volume Mount***
- Volume : In Pod Spec, user can define the storage volume available in for the Pod.
- Volume Specify the VolumeType and where the data is actually store.
- VolumeMount : VolumeMount in container spec, refer the Volume in Pod Spec and provide a MountPath.
```bash
apiVersion: v1
kind: Pod
metadata: 
  name: volume-mount-pod
spec:
  volumes:
    - name: my-sample-vol
      hostPath:
        path: /data
    containers:
      - name: volume-mount-pod
        image: nginx:latest
        command: ["/bin/sh", "-c", "echo Kubernetes DevOps"]
        volumeMounts:
          - name: volume-mount-pod
            mountPath: /output
```

***emptyDir Volume***
- emptyDir : emptyDir created when Pod is assigned to Node and Persist as long as Pod running on the Node.
- Multiple containers can refer the same emptyDir Volume.
- Multiple containers in the Pod can read and write the same files in the emptyDir volume, though that volume can be mounted at the same or different paths in each container.
```bash
apiVersion: v1
kind: Pod
metadata: 
  name: volume-mount-pod
spec:
  volumes:
    - name: my-cache-vol
      emptyDir: {}
    containers:
      - name: volume-mount-pod
        image: nginx:latest
        command: ["/bin/sh", "-c", "echo Kubernetes DevOps"]
        volumeMounts:
          - name: my-cache-vol
            mountPath: /cache
```
***Share Volume***
- User can use the same volumeMounts to share the same Volume to multiple container within the Same Pod.
- This is very powerful feature which can be used to data transformation of Data Processing.
- hostPath & emtpyDir volumeType support share volumes.
```bash
apiVersion: v1
kind: Pod
metadata: 
  name: volume-mount-pod
spec:
  volumes:
    - name: my-cache-vol
      emptyDir: {}
    containers:
      - name: volume-mount-pod
        image: nginx:latest
        command: ["/bin/sh", "-c", "echo Kubernetes DevOps"]
        volumeMounts:
          - name: my-cache-vol
            mountPath: /cache
    containers:
      - name: volume-mount-pod-1
        image: nginx:latest
        command: ["/bin/sh", "-c", "echo Kubernetes DevOps"]
        volumeMounts:
          - name: my-cache-vol
            mountPath: /cache/tmp
```
Host Path Volume
```bash
ls /var/tmp
kubectl apply -f hostpath-volume-pod.yaml
kubectl get pod
ls /var/tmp # see output.txt
```
NB: Delete/Remove of the pod, the file output.txt remain unchanged.

emptyDir
```bash
kubectl apply -f empty-dir-vol.yaml
kubectl get pod
kubectl get pod -o wide # see mount point
kubectl describe pod/redis-emptydir
kubectl exec -it redis-emptydir -- /bin/bash
ls # see redis directory
ls redis # see redis directory
cd /data/redis
echo "Hello, Kubernetes How are you" >> index.txt
cat index.txt
exit
kubectl delete pod redis-emptydir
kubectl apply -f empty-dir-vol.yaml
kubectl exec -it redis-emptydir -- /bin/bash
cd /data/redis # not index.txt available
touch testfile.txt
ps -aux # if it not work
apt-get update
apt-get install procps
ps -aux # delete the pod then only delete the emptyDir. And Delete/Destroy/Remove the container the emptyDir will remain unchanged.
kubectl get pod redis-emptydir --watch # run it another tab of terminal
kill 1 # kill the redis process, it means kil the container & run again
ls redis # see testfile.txt is exist
```

Shared/Common Volume
```bash
kubectl apply -f shared-volume.yaml
kubectl get pod
kubectl describe pod/shared-multi-container # check mount point
kubectl get pod -o wide
kubectl exec -it shared-multi-container -- /bin/bash
cd /usr/share/nginx/html
cat index.html
echo "welcome to nginx server"
```

***Persistent Volumes***
- PersistentVolumes are k8s Object that allow user to treat Storage as an Abstract Resource.
- PV is resource in the cluster just like a node is a cluster resource.
- PV uses a set of Attribute to describe the underlying storage resources (Disk or Cloud Storage), which will be used to store data.
Sample manifest
```bash
apiVersion: v1
kind: PersistentVolume
metadata:
  - name: static-persistent-volume
spec:
  capacity:
    storage: 1Gi
  accessModes:
    - ReadWriteMany
  hostPath:
    path: /var/tmp
  storageClassName: local-storage  
```

***Storage Classes***
- StorageClass allows K8s Administrator to Specify all type of Storage Service they offer on their Platform.
- Admin cloud create a StorageClass called Slow to describe inexpensive storage for general Development use.
- Admin cloud create a StorageClass called Fast for High I/O Operation Applications.
Sample manifest
```bash
apiVersion: storage.k8s.io/v1
kind: StorageClass
metadata:
  name: local-storage
provisioner: kubernetes.io/no-provisioner
volumeBindingMode: WaitForFirstConsumer
```
```bash
kubectl apply -f localhost-storage-class.yaml
kubectl describe storageclass.storage.k8s.io/local-storage
```
Slow Storage Class
```bash
apiVersion: storage.k8s.io/v1
kind: StorageClass
metadata:
  name: slow
provisioner: kubernetes.io/aws-ebs
parameters:
  type: io1
  iopsPerGB: "10"
  fsType: ext4
```
Fast Storage Class
```bash
apiVersion: storage.k8s.io/v1
kind: StorageClass
metadata:
  name: fast
provisioner: kubernetes.io/aws-ebs
parameters:
  type: gp2
  fsType: ext4
allowedTopologies:
  - matchLabelExpressions:
      - key: topology.kubernetes.io/zone
        values:
          - us-east-1a
```

***allowVolume Expansion***
- allowVolumeExpansion - This field can accept boolean value only.
- This is the property of StorageClass and define whether StorageClass supports the ability to resize after they are created.
- All Cloud Disk Supports this property.
Sample manifest
```bash
apiVersion: storage.k8s.io/v1
kind: StorageClass
metadata:
  name: local-storage
provisioner: kubernetes.io/no-provisioner
volumeBindingMode: WaitForFirstConsumer
allowVolumeExpansion: true  
```

***Reclaim Policy***
- persistentVolumeReclaimPolicy - This define, how the storage will be reused, when the PVs associated PVCs are deleted.
- Retain - Keep all the data. This require manual data cleanup and prepare for reuse.
- Delete - Delete underlying storage resources automatically (Support for Cloud Resource Only).
- Recycle - Automatically delete all data in underlying storage. Allow PVs to be reuse.
Sample manifest
```bash
apiVersion: v1
kind: PersistentVolume
metadata:
  name: static-persistent-volume
spec:
  capacity:
    storage: 1Gi
  accessModes:
    - ReadWriteMany
  hostPath:
    path: /var/tmp
  storageClassName: local-storage
  persistentVolumeReclaimPolicy: Recycle
```

***PersistentVolumeClaim***
- PersistentVolumeClaim (PVC) is a request for storage by a user.
- PVCs define a set of attribute Similar to those of PVs.
- PVCs look for a PVs that is able to meet the criteria. If it found one, will automatically be bound to that PV.
Sample manifest
```bash
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: static-pvc
spec:
  accessModes:
    - ReadWriteMany
  resources:
    requests:
      storage: 1Gi
  storageClassName: local-storage
```
```bash
kubectl apply -f persistent-volume.yaml
kubectl describe persistentvolume/persistent-volume
kubectl get pv -o wide
```
```bash
kubectl apply -f persistent-volume-claim.yaml
kubectl describe persistentvolumeclaim/static-pvc
kubectl get pv -o wide
kubectl get pvc -o wide # status pending due to first consumer
```
Checking activities of PVC in details
```bash
kubectl apply -f persistent-volume-pod.yaml
kubectl describe pod/pvc-pod
kubectl get pv -o wide # see persistent volume created, status bound & claim is default/static-pvc
kubectl get pvc -o wide # see status bound (earlier was pending) & cname is static-pvc
kubectl edit pvc static-pvc # here static-pvc is pvc name. change the storage
kubectl delete -f persistent-volume-pod.yaml
kubectl get pvc -o wide # here static-pvc is pvc name > still unchanged
kubectl delete -f persistent-volume-claim.yaml
kubectl get pv -o wide # status change bound to available
```