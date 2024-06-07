
Resource scheduling in Kubernetes involves allocating resources such as CPU and memory to containers and ensuring that Pods are optimally placed on nodes within a cluster. Kubernetes uses a scheduler to determine which node an unscheduled Pod should run on based on resource requests, constraints, and policies.

Ways of resources allocation or scheduling
- Node Selector
- Node Affinity & Anti-Affinity
- Pod Affinity & Anti-Affinity
- Taints & Tolerations
- Resource Requests and Limits
- Priority Classes
  
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
kubectl label nodes worker1 disktype=ssd
kubectl get pod
kubectl get pod -o wide
```
Pod schedule to specified node using `node-name`
```bash
kubectl get pod

```