
Resource scheduling in Kubernetes involves allocating resources such as CPU and memory to containers and ensuring that Pods are optimally placed on nodes within a cluster. Kubernetes uses a scheduler to determine which node an unscheduled Pod should run on based on resource requests, constraints, and policies.

Ways of resources allocation or scheduling
- Node Selector
- Node Affinity & Anti-Affinity
- Taints & Tolerations
- Resource Requests and Limits
- [Priority Classes](https://kubernetes.io/docs/concepts/scheduling-eviction/pod-priority-preemption/#priorityclass)

**Node Selector** allows you to assign  pods to specific node that have specific labels & its working in key-value pairs. For example `disktype:ssd` or `env=production` etc. Limitations
- Static Scheduling
- Lack of Affinity/Anti-Affinity Rules

**Node Affinity & Anti-Affinity** A more expressive and flexible way to control pod scheduling based on node labels. Node affinity supports `requiredDuringSchedulingIgnoredDuringExecution` (similar to nodeSelector) and `preferredDuringSchedulingIgnoredDuringExecution` (soft rules or preferences).

**Taints & Tolerations**: Taints and Tolerations are concepts in Kubernetes that help control the scheduling of pods to nodes, ensuring that certain workloads run on specific nodes or are prevented from running on unsuitable ones.
- **Taints** are markers applied to nodes that repel certain pods from being scheduled on them.
- **Tolerations** are properties applied to pods that allow them to be scheduled on nodes with specific taints.
  
Scheduling Process
- Filtering
- Scoring
- Binding

For fulfilling this section we need a master node & at least two nodes.

Pod schedule to specified node using `node-selector`
```bash
kubectl get pod
kubectl get pod -o wide
kubectl get nodes --show-labels
kubectl label nodes nodeName assignLabel # assign expected label to node
kubectl label nodes workerone-plane disktype=ssd
kubectl get pod
kubectl get pod -o wide
```

DaemonSet: 

A DaemonSet in Kubernetes is a controller that ensures a copy of a specified pod runs on all (or some) nodes in the cluster. It is particularly useful for deploying system-level services and background tasks that need to run on every node.
- When a new node is added to the cluster, the DaemonSet automatically deploys the pod on the new node.
- When a node is removed from the cluster, the pod managed by the DaemonSet is also removed.
- Updating the DaemonSet template triggers a rolling update across all nodes where the DaemonSet is deployed.
- Supports the ability to perform rolling updates to update pods incrementally.

Use Cases
- ***System Monitoring:*** Deploying monitoring agents like Prometheus Node Exporter or DataDog agents to gather metrics from every node.
- ***Log Collection:*** Running logging agents like Fluentd or Logstash on each node to collect and forward logs.
- ***Networking:*** Managing network plugins like Calico or Weave Net that need to run on every node for consistent network policy enforcement.
- ***Security:*** Deploying security agents like Falco to monitor and enforce security policies across the cluster.