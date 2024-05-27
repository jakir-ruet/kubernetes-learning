#### Lets me check whether any pod available in cluster
```bash
kubectl get pods -o wide
```
#### create 'my-draining-pod.yaml' for pod
```bash
kubectl apply -f my-draining-pod.yaml
```
#### create 'deployment.yaml'
```bash
kubectl apply -f deployment.yaml
kubectl get pods -o wide
```
#### getting an error due to pod available
```bash
kubectl drain worker1
```
#### got an error due to pod available
```bash
kubectl drain worker1 --ignore-daemonsets
```
#### got an error
```bash
kubectl drain worker1 --ignore-daemonsets --force
kubectl get pods -o wide
kubectl uncordon worker1
kubectl get node
kubectl get pods -o wide
```