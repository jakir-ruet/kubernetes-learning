Application scaling
- app scaling
  - vertical scaling (up-down)
  - horizontal scaling (left-right)
- stateless application
  - not save any data
  - not maintain the state for next state
  - can be scaled horizontally
- stateful application
  - save data (cannot split multiple instances)
  - maintain the state for next state
  - can be scaled vertically
- replication controller
  - manage the app scaling
  - ensure the specified of pod replicas are running at any point of time
  - ensure a pod/set of pod is always up & available

Horizontal scale
```bash
kubectl scale --replicas=numberOfPod replicationcontroller/repicationControllerName
# here create more 3 pods (previous 3 + 3)
kubectl scale --replicas=6 replicationcontroller/my-replica-controller
# descale I mean remove 3 recently created pods
kubectl scale --replicas=3 replicationcontroller/my-replica-controller
# delete all pod using this command
kubectl delete -f replication-controller.yaml
``