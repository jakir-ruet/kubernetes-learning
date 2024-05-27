# Lets me check whether any pod available in cluster
`kubectl get pods -o wide`
# create 'my-draining-pod.yaml' for pod
`kubectl apply -f my-draining-pod.yaml`
# create 'deployment.yaml'
`kubectl apply -f deployment.yaml`
`kubectl get pods -o wide`
`kubectl drain worker1` # getting an error due to pod available
`kubectl drain worker1 --ignore-daemonsets` # got an error due to pod available
`kubectl drain worker1 --ignore-daemonsets --force` # got an error
`kubectl get pods -o wide`
`kubectl uncordon worker1`
`kubectl get node`
`kubectl get pods -o wide`