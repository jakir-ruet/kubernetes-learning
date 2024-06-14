[Storage](https://kubernetes.io/docs/concepts/storage/) 

Container File System
- Container File System is ephemeral.
- Files in container File System Exists only as long as the Container Exists.
- Data in container File System is lost as soon as Container Deleted or recreated.

***Volumes***
- Many Application needs a persistent Data.
- Volumes allows to store Data Outside the Container, while allow container to Access Data at RunTime.

***Persistent Volumes***
- Volumes offer a way to provide external storage to container within the Pod/Container Specification.
- Persistent Volumes are a bit more advanced than Volumes.
- Persistent Volumes allow user to treat Storage as an Abstract Resource and consume it using Pods.

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

***emptyDir Volume***
- emptyDir : emptyDir created when Pod is assigned to Node and Persist as long as Pod running on the Node.
- Multiple containers can refer the same emptyDir Volume.
- Multiple containers in the Pod can read and write the same files in the emptyDir volume, though that volume can be mounted at the same or different paths in each container.

***Share Volume***
- User can use the same volumeMounts to share the same Volume to multiple container within the Same Pod.
- This is very powerful feature which can be used to data transformation of Data Processing.
- hostPath & emtpyDir volumeType support share volumes.

***Persistent Volumes***
- PersistentVolumes are k8s Object that allow user to treat Storage as an Abstract Resource.
- PV is resource in the cluster just like a node is a cluster resource.
- PV uses a set of Attribute to describe the underlying storage resources (Disk or Cloud Storage), which will be used to store data.

***Storage Classes***
- StorageClass allows K8s Administrator to Specify all type of Storage Service they offer on their Platform.
- Admin cloud create a StorageClass called Slow to describe inexpensive storage for general Development use.
- Admin cloud create a StorageClass called Fast for High I/O Operation Applications.

***allowVolume Expansion***
- allowVolumeExpansion - This field can accept boolean value only.
- This is the property of StorageClass and define whether StorageClass supports the ability to resize after they are created.
- All Cloud Disk Supports this property.

***Reclaim Policy***
- persistentVolumeReclaimPolicy - This define, how the storage will be reused, when the PVs associated PVCs are deleted.
- Retain - Keep all the data. This require manual data cleanup and prepare for reuse.
- Delete - Delete underlying storage resources automatically (Support for Cloud Resource Only).
- Recycle - Automatically delete all data in underlying storage. Allow PVs to be reuse.

***PersistentVolumeClaim***
- PersistentVolumeClaim (PVC) is a request for storage by a user.
- PVCs define a set of attribute Similar to those of PVs.
- PVCs look for a PVs that is able to meet the criteria. If it found one, will automatically be bound to that PV.