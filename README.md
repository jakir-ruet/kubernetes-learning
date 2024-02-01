[![LinkedIn][linkedin-shield-lapissoft]][linkedin-url-lapissoft]
[![Facebook-Page][facebook-shield-lapissoft]][facebook-url-lapissoft]
[![Youtube][youtube-shield-lapissoft]][youtube-url-lapissoft]

## Visit Us [Lapis Soft](http://www.lapissoft.com)

### Docker

Docker is a platform and set of tools designed to facilitate the creation, deployment, and running of applications in lightweight, portable containers. Containers allow developers to package an application and its dependencies, including libraries and other components, into a single, standardized unit. This unit can then be easily moved between different environments, such as development, testing, and production, without worrying about differences in the underlying infrastructure.

### Types of Processes

There are fundamentally two types of processes in Unix based OS:

#### Foreground processes

These are initialized and controlled through a terminal session (referred to as interactive processes). In other words, there has to be a user connected to the system to start such processes; they haven’t started automatically as part of the system functions/services.

#### Background processes

Are processes not connected to a terminal (referred to as non-interactive/automatic processes); they don’t expect any user input.

### Attached-Detached

We can run container in attached mode (in the foreground) or in detached mode (in the background). By default, Docker runs the container in attached mode. In the attached mode, Docker can start the process in the container and attach the console to the process's standard input, standard output, and standard error.

#### Docker Installation

- How to [Install?](https://docs.docker.com/get-docker/). You may install as per your operating system.

#### How to pull from Docker Hub

- `docker pull jakirbd/doc-kub-first-app:latest`

#### Run the downloaded docker image & access to the application

- `docker run --name AppName -p 3000:80 -d UserName/AppName:TagName`
- `docker run --name doc-kub-first-app -p 3000:80 -d jakirbd/doc-kub-first-app:latest`
- `docker exec -it AppName /bin/sh`
- `docker exec -it doc-kub-first-app /bin/sh`

#### Essential Commands of Docker

|  SL   | Command                 | Explanation                    |
| :---: | :---------------------- | :----------------------------- |
|   1   | `docker --version (-v)` | Checking the version of docker |
|   2   | `docker login`          | We can access using credential |
|   3   | `docker logout`         | We can logout                  |

#### Essential command of images

|  SL   | Command                                          | Explanation                                                 |
| :---: | :----------------------------------------------- | :---------------------------------------------------------- |
|   1   | `docker image`                                   | Show the command details                                    |
|   2   | `docker images or docker ls`                     | Show image list                                             |
|   3   | `docker pull ImageName`                          | Pull/Download the image                                     |
|   4   | `docker pull ImageName:TagName`                  | Pull/Download the image with tag name                       |
|   5   | `docker run ImageName (node/nginx)`              | Will be Run & Publish a new container for each publish      |
|   6   | `docker run -it ImageName (node/nginx)`          | Enter into the interactive mode                             |
|   7   | `docker build -t doc-kub-first-app:latest .`     | Build the images with tag (name/version/others) (own image) |
|   8   | `docker image tag ImgId UserName/ImgName:latest` | Image renaming/taging (own image)                           |
|   9   | `docker push jakirbd/my-node-server`             | Pushing the image (own image)                               |
|  10   | `docker image history ImageId`                   | History of image                                            |
|  11   | `docker image inspect ImageId`                   | Inspections the image                                       |
|  12   | `docker image prune -a`                          | Remove all unused images, not just dangling ones            |
|  13   | `docker rmi ImageId`                             | Image remove                                                |

#### Essential command of container

|  SL   | Command                                                               | Explanation                                            |
| :---: | :-------------------------------------------------------------------- | :----------------------------------------------------- |
|   1   | `docker container`                                                    | Show the command details                               |
|   2   | `docker container ls`                                                 | Show the enlisted container                            |
|   3   | `docker ps`                                                           | Show only running container                            |
|   4   | `docker ps -a`                                                        | Show all container                                     |
|   5   | `docker ps -a -q`                                                     | Show all container with id (quiet)                     |
|   6   | `docker build .`                                                      | Build a container                                      |
|   7   | `docker build -t TagName .`                                           | Build a container with tag                             |
|   8   | `docker run -p 3000:80 nginx/node/https`                              | Will be Run & Publish a new container for each publish |
|   7   | `docker run -p 3000:80 BaseImageId`                                   | Will be Run & Publish a new container for each publish |
|   9   | `docker rename OldContName NewContName`                               | Renaming the container                                 |
|  10   | `docker run -p 3000:80 -d --name NewContName OldContName`             | Renaming & publishing container                        |
|  11   | `docker run -p 3000:80 -d --rm --name NewContName OldContName`        | Renaming, removing & publishing container              |
|  12   | `docker run -p 3000:80 -d --rm --name NewContName OldContName:latest` | Renaming, removing & publishing using tag container    |
|  13   | `docker run -p 3000:80 -d BaseImageId`                                | Publish the container as detach                        |
|  14   | `docker run -p 3000:80 -d --rm BaseImageId`                           | Container is Remove after stop the container           |
|  15   | `docker exec -it ContainerName /bin/sh`                               | Container connect to terminal using shell              |
|  16   | `docker exec -it ContainerName /bin/bash`                             | Container connect to terminal using bash               |
|  17   | `docker exec -it ContainerName /bash`                                 | Container connect to terminal using bash               |
|  18   | `docker cp index.html my-nginx-server:/usr/share/nginx/html`          | Moving the source file local pc to docker nginx server |
|  19   | `docker container prune`                                              | Remove all container                                   |
|  20   | `docker start ContainerName`                                          | Container start                                        |
|  21   | `docker stop ContainerName`                                           | Container stop                                         |
|  22   | `docker restart ContainerName`                                        | Container restart                                      |
|  23   | `docker rm ContainerName`                                             | Container remove after stop it                         |
|  24   | `docker attach ContainerName`                                         | Attach the container                                   |
|  25   | `docker logs ContainerName`                                           | See the logs details                                   |
|  26   | `docker logs -f ContainerName`                                        | See the future logs details                            |

#### Data-Storage

##### Data

- Application Data (Code, dependencies, package.json Environment)

  - written by developer
  - added to image & container in build phase.
  - read-only/fixed once image is build

- Temporary App Data (Generated data, Enter user input into form)

  - fetched/produced in running container
  - stored in memory or temporary files
  - read + write possible temporary stored in containers

- Permanent App Data (User accounts)

  - fetched/produced in running container
  - stored in files or a database, must not lost container stop/restart
  - read + write possible permanent containers & volumes

##### Storage

- Volumes (managed by docker)
  - Anonymous Volumes
  - Named Volumes
- Bind/Host Mounts (managed by we)
- Manage data in [Docker](https://docs.docker.com/storage/)

#### Essential command of volume

|  SL   | Command                                                                  | Explanation                                                  |
| :---: | :----------------------------------------------------------------------- | :----------------------------------------------------------- |
|   1   | `docker volume create`                                                   | Create a anonymous volume                                    |
|   2   | `docker volume create my-sweet-vol`                                      | Create a volume                                              |
|   3   | `docker volume ls`                                                       | Check the volume list                                        |
|   4   | `docker volume inspect VolName`                                          | Inspect the volume                                           |
|   5   | `docker volume rm VolName`                                               | Remove the volume                                            |
|   6   | `docker volume prune`                                                    | Remove the anonymous volume                                  |
|   7   | `docker build -t ImgName(OldContName):volumes .`                         | Create own images tag named volumes                          |
|   8   | `docker run -d -p 3000:80 --rm --name NewContName OldContName:volumes`   | Create own images tag named based on volumes                 |
|   9   | `docker rmi ConName:volumes`                                             | Remove the named volume                                      |
|  10   | `docker run -it --name ConName -v /DirName nginx /bin/bash`              | Create a container & anonymous volume mounted on a directory |
|  11   | `docker run -it --name ConName -v VolName:/DirName nginx /bin/bash`      | Create a container & named volume mounted on a directory     |
|  12   | `mkdir /opt/HostDir`                                                     | Create host directory use as volume for app                  |
|  13   | `docker run -it --name ConName -v /opt/HostDir:/HostDir nginx /bin/bash` | Create a image, container on host directory                  |

#### Footnote about volume

- Storage persistent location outside of container.
- If container removed then volume will be available on storage.
- It use for the data security purpose.

#### Network in [Docker](https://docs.docker.com/network/)

##### Networks types

- Bridge Network
- User Define Bridge Network
- Host Network (under main OS)

|  SL   | Command                                                          | Explanation                                         |
| :---: | :--------------------------------------------------------------- | :-------------------------------------------------- |
|   1   | `docker network ls`                                              | Check the network list                              |
|   2   | `ip address show`                                                | Check IP address in terminal; it will show docker0: |
|   3   | `ip add sh`                                                      | Show all network interface in details               |
|   4   | `bridge link`                                                    | Show all ethernet name and connected docker         |
|   5   | `docker inspect bridge`                                          | Inspect all bridge networks with individual IP      |
|   6   | `docker exec -it ImageName sh`                                   | Enter into image                                    |
|   7   | `ip route`                                                       | Show IP, DNS & others                               |
|   8   | `docker run -itd --rm -p 85:80 --name myforthapp nginx`          | Run a nginx app                                     |
|   9   | `docker network create NetworkName`                              | Create a network, default bridge                    |
|  10   | `docker network create -d NetworkType NetworkName`               | Create a network, assign network type & name        |
|  11   | `docker network inspect NetworkName`                             | Inspect the user define network                     |
|  12   | `docker run -itd --rm --network NetworkName --name loki ImgName` | Create an user define network under NetworkName     |
|  13   | `docker inspect NetworkName`                                     | Inspect the user define network under NetworkName   |
|  14   | `docker inspect NetworkName`                                     | Inspect the user define network under NetworkName   |

##### Network Driver Types

- **_bridge_** The default network driver. If you don't specify a driver, this is the type of network you are creating. Bridge networks are commonly used when your application runs in a container that needs to communicate with other containers on the same host.
- **_host_** Remove network isolation between the container and the Docker host, and use the host's networking directly.
- **_overlay_** Overlay networks connect multiple Docker daemons together and enable Swarm services and containers to communicate across nodes. This strategy removes the need to do OS-level routing.
- **_ipvlan_** IPvlan networks give users total control over both IPv4 and IPv6 addressing. The VLAN driver builds on top of that in giving operators complete control of layer 2 VLAN tagging and even IPvlan L3 routing for users interested in underlay network integration.
- **_macvlan_** Macvlan networks allow you to assign a MAC address to a container, making it appear as a physical device on your network. The Docker daemon routes traffic to containers by their MAC addresses. Using the macvlan driver is sometimes the best choice when dealing with legacy applications that expect to be directly connected to the physical network, rather than routed through the Docker host's network stack.

### Kubernetes

Kubernetes, often abbreviated as K8s, is an open-source container orchestration platform that automates the deployment, scaling, and management of containerized applications. It was originally developed by Google and is now maintained by the Cloud Native Computing Foundation (CNCF). Kubernetes provides a powerful and flexible platform for container orchestration, allowing you to deploy and manage applications seamlessly across a cluster of machines.

##### Cluster

It is made up of at least one master node and one or more worker nodes. The **_master node makes up the control plane_** of a cluster and is responsible for scheduling tasks and monitoring the state of the cluster.
![Cluster](/img/cluster.png)

#### Components

A Kubernetes cluster consists of a set of worker machines, called **_nodes (vm)_**, that run containerized applications. Every cluster has at least **_one worker_** node (vm).

##### [Types of components](https://kubernetes.io/docs/concepts/overview/components/)
- Control Plane Components
  - kube-apiserver
  - etcd
  - kube-scheduler
  - kube-controller-manager
    - node-controller
    - replication-controller
    - endpoint-controller
    - service-account-controller
  - cloud-controller-manager
    - node-controller
    - route-controller
    - service-controller 
- Worker Node Components
  - kubelet
  - kube-proxy
  - container runtime
- Kubernetes
  - DNS
  - Kubernetes Dashboard
  - Resource Monitoring
  - Logging

***Services***  is a logical abstraction for a deployed group of pods in a cluster (which all perform the same function). if one is crash then another will ready to work. The core attributes of a Kubernetes service are:
- A label selector that locates pods
- The clusterIP IP address and assigned port number
- Port definitions
- Optional mapping of incoming ports to a targetPort

***Types of service***

***ClusterIP:***
Exposes a service which is only accessible from within the cluster. It is the default type of service, which is used to expose a service on an IP address internal to the cluster. Access is only permitted from within the cluster.

***NodePort*** 
Exposes a service via a static port on each node’s IP.

***LoadBalancer*** 
Exposes the service via the cloud provider’s load balancer like AWS or Azure.

***ExternalName*** 
Maps a service to a predefined externalName field by returning a value for the CNAME record.

***Sample Service***
``` YAML
apiVersion: v1
kind: Service
metadata:
  name: my-service
spec:
  selector:
    app.kubernetes.io/name: MyApp
  ports:
    - protocol: TCP
      port: 80
      targetPort: 9376
```

##### Pods (containers)

Pods are the smallest deployable units of computing that you can create and manage in Kubernetes. A Pod (as in a pod of whales or pea pod) is a group of **_one or more containers_**, with shared **_storage_** and **_network resources_**, and a **_specification_** for how to run the containers. A Pod's contents are always co-located and co-scheduled, and run in a shared context.

``` YAML
  apiVersion: v1
  kind: Pod
  metadata:
    name: nginx
  spec:
    containers:
    - name: nginx
      image: nginx:1.14.2
      ports:
      - containerPort: 80
```

##### Nodes

A node may be a virtual or physical machine, depending on the cluster. Each node is managed by the control plane and contains the services necessary to run Pods.

```JSON
{
  "kind": "Node",
  "apiVersion": "v1",
  "metadata": {
    "name": "10.240.79.157",
    "labels": {
      "name": "my-first-k8s-node"
    }
  }
}
```

A Kubernetes cluster is made up of one **_master_** node and several **_worker_** nodes. The worker nodes are responsible for running the containers and doing any work assigned to them by the master node. The master node looks after or manage the cluster.

- Master Node

  - **_kube-apiserver (cluster gateway) (some request > apiserver > validates request > others process > pod):_** It is entry-point to the Kubernetes cluster, which itself is a container. This is the process that allows communication between different Kubernetes clients and the cluster. We can run several instances of kube-apiserver and balance traffic between those instances. It is designed to scale horizontally—that is, it scales by deploying more instances. it is working as gatekeeper for authentication.
  - **_etcd (cluster brain):_** is a distributed key-value store that plays a critical role in the architecture of Kubernetes. Consistent and highly-available key value store used as Kubernetes' backing store for all cluster data. If your Kubernetes cluster uses etcd as its backing store, make sure you have a back up plan for the data. application data is not stored in etcd.
  - **_kube scheduler (schedule new pod > apiserver > scheduler > where put pod > kubelet):_** Scheduler ensures proper pod placement on the worker nodes based on several factors such as the available resources and the current load on the cluster.
  - **_kube controller manager (kcm > scheduler > kubelet > pod):_** Logically, each controller is a separate process, but to reduce complexity, they are all compiled into a single binary and run in a single process. its can detects cluster state changes. There are many different types of controllers some of them below;
    - **_Node controller:_** Responsible for noticing and responding when nodes go down.
    - **_Job controller:_** Watches for Job objects that represent one-off tasks, then creates Pods to run those tasks to completion.
    - **_EndpointSlice controller:_** Populates EndpointSlice objects (to provide a link between Services and Pods).
    - **_ServiceAccount controller:_** Create default ServiceAccounts for new namespaces.
  - **_cloud controller manager:_** lets you link your cluster into your cloud provider's API, and separates out the components that interact with that cloud platform from components that only interact with your cluster.The following controllers can have cloud provider dependencies:
    - **_Node controller:_** For checking the cloud provider to determine if a node has been deleted in the cloud after it stops responding
    - **_Route controller:_** For setting up routes in the underlying cloud infrastructure
    - **_Service controller:_** For creating, updating and deleting cloud provider load balancers

- Worker Node: here three process shown below;

  - **_kubelet:_** An agent that runs on each node in the cluster. It makes sure that containers are running in a Pod. The kubelet takes a set of PodSpecs that are provided through various mechanisms and ensures that the containers described in those PodSpecs are running and healthy. The kubelet doesn't manage containers which were not created by Kubernetes.
  - **_kube proxy:_** kube-proxy is a network proxy that runs on each node in your cluster, implementing part of the Kubernetes Service concept. kube-proxy maintains network rules on nodes. These network rules allow network communication to your Pods from network sessions inside or outside of your cluster.
  - **_container runtime:_** A fundamental component that empowers Kubernetes to run containers effectively. It is responsible for managing the execution and lifecycle of containers within the Kubernetes environment.

***kubectl (kube control):*** is the command-line tool for interacting with Kubernetes clusters. It allows users to perform various operations on Kubernetes resources, such as creating and managing pods, services, deployments, and more.

***Minikube:*** is a tool that allows you to run a single-node Kubernetes cluster locally on your machine. It is designed to be a lightweight and easy-to-use solution for developers who want to develop, test, and experiment with Kubernetes applications without the need for a full-scale, multi-node cluster.

##### kubectl cli vs minikube cli?
kubectl and minikube are command-line tools used in the Kubernetes ecosystem, they serve different purposes. kubectl is a versatile tool for managing, configuring any Kubernetes cluster on minikube, while minikube is a tool specifically tailored for setting up, deleting and managing a local development cluster. You might use kubectl for broader Kubernetes management tasks, and minikube for local development and testing.
##### Install Minikube & kubectl (You may install as per your operating system.)
- Minikube [install](https://minikube.sigs.k8s.io/docs/start/) or Microk8s [install](https://microk8s.io) ***Minikube Recommended***. 
```bash 
  minikube start --force --driver=docker
```
Check the running/all container on docker
```bash 
  docker ps / docker ps -a
```
```bash 
  minikube dashboard
```
If it return "kubectl not found. If you need it, try: 'minikube kubectl -- get pods -A'", then
- You will Install [kubectl](https://kubernetes.io/docs/tasks/tools/install-kubectl-linux/) ***Ubuntu Server Recommended***.

|  SL   | Command                                                  | Explanation                            |
| :---: | :------------------------------------------------------- | :------------------------------------- |
|   1   | `kubectl -h`                                             | show all command                       |
|   2   | `kubectl get node`                                       | show enlisted node                     |
|   3   | `kubectl get pod`                                        | show enlisted pod                      |
|   4   | `kubectl get services`                                   | show enlisted services                 |
|   5   | `kubectl describe service serviceName`                   | show details of a service              |
|   6   | `kubectl get node -o wide`                               | show enlisted node in details          |
|   7   | `kubectl describe node`                                  | show description of node               |
|   8   | `kubectl top node NodeName`                              | move a node to top                     |
|   9   | `kubectl create deployment nginxDepltName --image=nginx` | nginx install on kubernetes            |
|  10   | `kubectl get deployment`                                 | show deployment list                   |
|  11   | `kubectl get pod`                                        | show enlisted pod                      |
|  12   | `kubectl describe pod podName`                           | description of node                    |
|  13   | `kubectl get replicaset`                                 | show replica set                       |
|  14   | `kubectl edit deployment nginxDepltName`                 | change deployment name (image version) |
|  15   | `kubectl delete deployment nginxDepltName`               | remove deployment                      |
|  16   | `kubectl logs podName`                                   | checking logs of a pod                 |
|  17   | `kubectl exec -it podName -- bin/bash`                   | debugging the pod                      |
|  18   | `kubectl apply -f config-file.yaml`                      | execute the conf file                  |
|  19   | `kubectl get pod --show-labels`                          | show the label of pod                  |
|  20   | `kubectl get pod -o yaml`                                | show yaml of pod                       |
|  21   | `kubectl get deployment deplName -o yaml`                | all info in output yaml file           |

#### First nginx deployment
|  SL   | Command                                  | Explanation                         |
| :---: | :--------------------------------------- | :---------------------------------- |
|   1   | `touch nginx-deployment.yaml`            | create yaml conf file on local pc   |
|   2   | `nano nginx-deployment.yaml`             | open in nano & write conf yaml code |
|   3   | `kubectl apply -f nginx-deployment.yaml` | deployment on the kubernetes        |
|   4   | `kubectl get deployment`                 | checking the deployment             |
|   5   | `kubectl exec -it podName -- bin/bash`   | accessing the pod                   |

##### See (2-yaml-file-parts) for yaml understanding. Recommended to read docs of [kubernetes](https://kubernetes.io/docs/home/).

##### See (3-complete-deployment) for complete deployment. Recommended to read docs of [kubernetes](https://kubernetes.io/docs/home/).

#### Ingress in [Docker](https://kubernetes.io/docs/concepts/services-networking/ingress/#:~:text=The%20Ingress%20concept%20lets%20you,define%20via%20the%20Kubernetes%20API.&text=An%20API%20object%20that%20manages,and%20name%2Dbased%20virtual%20hosting.)

Ingress exposes HTTP and HTTPS routes from outside the cluster to services within the cluster. Traffic routing is controlled by rules defined on the Ingress resource. Here is a simple example where an Ingress sends all its traffic to one Service:
![Ingress](/img/ingress.png)

``` YAML
  apiVersion: networking.k8s.io/v1
  kind: Ingress
  metadata:
    name: minimal-ingress
    annotations:
      nginx.ingress.kubernetes.io/rewrite-target: /
  spec:
    ingressClassName: nginx-example
    rules:
    - http:
        paths:
        - path: /testpath
          pathType: Prefix
          backend:
            service:
              name: test
              port:
                number: 80
```

|  SL   | Command                                        | Explanation                    |
| :---: | :--------------------------------------------- | :----------------------------- |
|   1   | `minikube addons enable ingress`               | install controller in Minikube |
|   2   | `kubectl apply -f dashboard-ingress.yaml`      | ingress create                 |
|   3   | `minikube get ingress -n kubernetes-dashboard` | see details of ingress         |

#### [Helm](https://helm.sh/docs/) | [Helm Cheat Sheet](https://helm.sh/docs/intro/cheatsheet/)
It is a package manager for Kubernetes applications. It simplifies the process of deploying and managing applications on Kubernetes clusters. Helm consists of two main components: the Helm client and the Helm server. The client is used to interact with the server and manage charts, while the server contains all the necessary information about available charts.
![Helm](/img/helm.png)

##### Helm Chart Architecture
![Helm](/img/helm-architecture.png)

##### Prerequisites
- A Kubernetes cluster
- Deciding what security configurations to apply to your installation, if any
- Installing and configuring Helm.

##### [Install](https://helm.sh/docs/intro/install/)
- You must have Kubernetes installed. For the latest release of Helm, we recommend the latest stable release of Kubernetes, which in most cases is the second-latest minor release.
- You should also have a local configured copy of kubectl. Or
- Download a binary release of the Helm client. You can use tools like homebrew, or look at the official releases page.

***Helm Chart:***
A Helm chart is a package of pre-configured Kubernetes resources, which are defined as templates. Helm is a package manager for Kubernetes applications, allowing you to define, install, and upgrade even the most complex Kubernetes applications. Chart structure show below;
- myHelmChart
  - charts
    - sub-chart & dependencies
  - templates
    - deployment.yaml
    - services.yaml
    - ingress.yaml
    - others kubernetes resources definitions
  - values.yaml
  - chart.yaml
  - README.md
  - LICENSE
  - Other files/directories

***Tiller (Server)***
In Helm, a package manager for Kubernetes, a "Tiller" refers to the server-side component of Helm. Helm follows a client-server architecture, where the Helm client interacts with the Tiller server to deploy and manage Kubernetes applications. Tiller is responsible for managing the release lifecycle, applying Kubernetes manifests, and keeping track of releases.

|  SL   | Command                           | Explanation                       |
| :---: | :-------------------------------- | :-------------------------------- |
|   1   | `kubectl get po --all-namespaces` | is namespaces running to confirm? |

##### Namespaces
Namespaces is virtual cluster in a cluster, where organized the resources. Namespaces provides a mechanism for isolating groups of resources within a single cluster. Names of resources need to be unique within a namespace, but not across namespaces. Kubernetes starts with four initial namespaces:

1. default

    - We can start using your new cluster without first creating a namespace.
    - Resource we can create are located here.

2. kube-node-lease

    - Heartbeats of nodes so that the control plane can detect node failure.
    - Each node has associated lease object in namespace.
    - Determines the availability of a node.

3. kube-public:

    - Publicly accessible data, even without any authentication.
    - A configure, which containers cluster information.
  
4. kube-system (```kubectl cluster-info```):

    - The namespace for objects created by the Kubernetes system.
    - Do not create or modify in kube system.
    - System Process.
    - Master and Kubectl processes

##### Importance

- Everything in one namespace (default).
  - Deployments
  - Replica sets
  - Services
  - Configmaps
- Resources grouping (database, monitoring, elastic stack, nginx-ingress) is possible in namespace.
- Conflicts minimization in same application with many teams.
- Resources sharing is possible such as staging, development, env setup.
- Limit the access into resource will possible on namespace.
- Own ConfigMap only possible in each namespace.

|  SL   | Command                                | Explanation               |
| :---: | :------------------------------------- | :------------------------ |
|   1   | `kubectl get namespaces`               | Check enlisted namespaces |
|   2   | `kubectl cluster-info`                 | Check the cluster info    |
|   3   | `kubectl create namespace myNamespace` | Create namespace          |

##### Volumes
It is a directory containing data, which can be accessed by containers in a Kubernetes pod. The location of the directory, the storage media that supports it, and its contents, depend on the specific type of volume being used. There are a few types of volumes in Kubernetes.

-------------------------
Sections
-------------------------
pod container-------------

- Volumes
  - Persistent Volumes (PV)
    is a piece of storage in the cluster that has been provisioned by an administrator or dynamically provisioned using Storage Classes. It is a resource in the cluster just like a node is a cluster resource. PVs are volume plugins like Volumes, but have a lifecycle independent of any individual Pod that uses the PV.
  - Persistent Volume Claim (PVC)
    is a request for storage by a user. It is similar to a Pod. Pods consume node resources and PVCs consume PV resources. Pods can request specific levels of resources (CPU and Memory). Claims can request specific size and access modes (e.g., they can be mounted ReadWriteOnce, ReadOnlyMany, 
  - Ephemeral Volumes
  - EmptyDir Volumes
  - hostPath Volumes
  - Volumes ConfigMap 
- [Storing Volumes](https://kubernetes.io/docs/concepts/storage/storage-classes/) 
  - NFS (Network File System)
  - CSI (Container Storage Interface)


ConfigMap and Secret------coming
StatefullSet deployment-----coming
Monitoring tools (Prometheus)-----coming

1. AWS CLI

   - Control multiple AWS services from this command line.
   - How to [Install?](https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html)
   - Let's me check `aws --version`
   - If its okay then we will see `aws-cli/2.15.4 Python/3.11.6 Darwin/23.2.0 exe/x86_64 prompt/off`
   - Configuration using security credential
     - Go to AWS Management Console > Services > IAM
     - Select the IAM User Name: Your User Name [_**NB**_: You must use IAM's Information only not Root User]
     - Click on `Security credentials`
     - Click on `Create access key`
     - Copy Access ID & Secret access key
     - Go to your Terminal and implement as below format
     - `aws configure`
     - AWS Access Key ID [None]: Put your ID here and press Enter.
     - AWS Secret Access Key [None]: Put your secret key here and press Enter
     - Default region name [None]: us-east-1
     - Default output format [None]: json
   - Let's me check whether the configuration is done.
     - `aws ec2 describe-vpcs`
     - If it is done then we will see the details of the default vpc.

2. kubectl

   - Control the kubernetes clusters & objects.
   - How to [Install?](https://docs.aws.amazon.com/eks/latest/userguide/install-kubectl.html)
   - `mkdir kubectlbinary`
   - `cd kubectlbinary`
   - `curl -O https://s3.us-west-2.amazonaws.com/amazon-eks/1.28.3/2023-11-14/bin/darwin/amd64/kubectl`
   - Assign the execute permissions `chmod +x ./kubectl`
   - Set the path by copying to user home directory `mkdir -p $HOME/bin && cp ./kubectl $HOME/bin/kubectl && export PATH=$HOME/bin:$PATH` & `echo 'export PATH=$HOME/bin:$PATH' >> ~/.bash_profile`

   - Let's me check whether the configuration is done. `kubectl version --client`
   - If it shows the following output then installation is done.
     - `Client Version: v1.28.2`
     - `Kustomize Version: v5.0.4-0.20230601165947-6ce0bf390ce3`

3. eksctl

   - creating-deleting clusters on AWS EKS.
   - create, autoscale & delete the node groups.
   - create fargate profiles.
   - it is powerfull tool for managing EKS clusters on AWS.
   - How to [Install?](https://docs.aws.amazon.com/emr/latest/EMR-on-EKS-DevelopmentGuide/setting-up-eksctl.html)
   - If you do not already have Homebrew installed on macOS, install it with the following command. `/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install.sh)"`
   - Install the Weaveworks Homebrew tap. `brew tap weaveworks/tap` or `brew install weaveworks/tap/eksctl`
   - Test that your installation was successful with the following command. You must have eksctl 0.34.0 version or later. `eksctl version`
   - If it shows the following output then installation is done.
   - `0.167.0`

## Courtesy of Jakir

[![LinkedIn][linkedin-shield-jakir]][linkedin-url-jakir]
[![Facebook-Page][facebook-shield-jakir]][facebook-url-jakir]
[![Youtube][youtube-shield-jakir]][youtube-url-jakir]

### Have a good day, stay with me
<!-- Personal profile -->

[linkedin-shield-jakir]: https://img.shields.io/badge/linkedin-%230077B5.svg?style=for-the-badge&logo=linkedin&logoColor=white
[linkedin-url-jakir]: https://www.linkedin.com/in/jakir-ruet/
[facebook-shield-jakir]: https://img.shields.io/badge/Facebook-%231877F2.svg?style=for-the-badge&logo=Facebook&logoColor=white
[facebook-url-jakir]: https://www.facebook.com/jakir-ruet/
[youtube-shield-jakir]: https://img.shields.io/badge/YouTube-%23FF0000.svg?style=for-the-badge&logo=YouTube&logoColor=white
[youtube-url-jakir]: https://www.youtube.com/@mjakaria-ruet/featured

<!-- Company profile -->

[linkedin-shield-lapissoft]: https://img.shields.io/badge/linkedin-%230077B5.svg?style=for-the-badge&logo=linkedin&logoColor=white
[linkedin-url-lapissoft]: https://www.linkedin.com/company/lapis-soft/
[facebook-shield-lapissoft]: https://img.shields.io/badge/Facebook-%231877F2.svg?style=for-the-badge&logo=Facebook&logoColor=white
[facebook-url-lapissoft]: https://www.facebook.com/GoLapisSoft/
[youtube-shield-lapissoft]: https://img.shields.io/badge/YouTube-%23FF0000.svg?style=for-the-badge&logo=YouTube&logoColor=white
[youtube-url-lapissoft]: https://www.youtube.com/@LapisSoft/featured
