Upgrade the image in the deployment `my-deployment` to `busybox:1.28`, check the history and roll back.
`Answer`
```bash
kubectl describe deployment my-deployment | grep Image
kubectl set image deployment my-deployment busybox=busybox:1.28 --record
kubectl describe deployment my-deployment | grep Image
kubectl rollout undo deployment my-deployment
kubectl describe deployment my-deployment | grep Image
```

Rollback the deployment `my-deployment` to revision 1.
`Answer`
```bash
kubectl rollout history deployment my-deployment
kubectl rollout history deployment my-deployment --revision=1
```

Scale the `my-deployment` deployment to 6 replicas.
`Answer`
```bash
kubectl get pods
kubectl describe deployment my-deployment
kubectl scale deployment my-deployment --replicas=6
kubectl get pods
kubectl describe deployment my-deployment
```

Expose the deployment `my-deployment`.
- name service: my-service
- port: 6379
- type: NodePort
`Answer`
```bash
kubectl get deployment my-deployment -o wide
kubectl expose deployment my-deployment --type=NodePort --port=6379 --target-port=6379 --name=my-service
kubectl get services
kubectl describe service my-service
```