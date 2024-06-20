#### Lets me check whether any pod available in cluster
In Kubernetes, "draining" a node refers to the process of safely evicting all the pods from a node in preparation for maintenance, decommissioning, or other administrative tasks. Draining ensures that the node can be taken out of service without disrupting the overall operation of the Kubernetes cluster.

```bash
kubectl get pods -o wide
# create 'my-draining-pod.yaml' for pod
kubectl apply -f my-draining-pod.yaml 
# create 'deployment.yaml'
kubectl apply -f deployment.yaml
kubectl get pods -o wide
kubectl drain worker1 # getting an error due to pod available
kubectl drain worker1 --ignore-daemonsets # got an error due to pod available
kubectl drain worker1 --ignore-daemonsets --force # got an error
kubectl get pods -o wide
kubectl uncordon worker1
kubectl get node
kubectl get pods -o wide
```
| Command    | Purpose                           | Effect                                                         | Command Example                                                     | Typical Use Cases                                        |
| ---------- | --------------------------------- | -------------------------------------------------------------- | ------------------------------------------------------------------- | -------------------------------------------------------- |
| `cordon`   | Mark a node as unschedulable      | Prevents new pods from being scheduled on the node             | `kubectl cordon <node-name>`                                        | Preparing for maintenance, testing without new workloads |
| `uncordon` | Mark a node as schedulable        | Allows new pods to be scheduled on the node                    | `kubectl uncordon <node-name>`                                      | Re-enabling a node after maintenance, scaling up         |
| `drain`    | Safely evict all pods from a node | Evicts all non-DaemonSet pods, respecting PodDisruptionBudgets | `kubectl drain <node-name> --ignore-daemonsets --delete-local-data` | Node decommissioning, maintenance, troubleshooting       |
