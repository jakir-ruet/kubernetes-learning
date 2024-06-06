
Resource scheduling in Kubernetes involves allocating resources such as CPU and memory to containers and ensuring that Pods are optimally placed on nodes within a cluster. Kubernetes uses a scheduler to determine which node an unscheduled Pod should run on based on resource requests, constraints, and policies.

Ways of resources allocation or scheduling
- Resources & Limits
  - Resource requests
  - Resource Limits
- Node Selector
- Node Affinity & Anti-Affinity
- Pod Affinity & Anti-Affinity
- Taints & Tolerations
- Resource Quotas
  
  Scheduling Process
  - Filtering
  - Scoring
  - Binding