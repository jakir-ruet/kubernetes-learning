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
```

ReplicaSet
- ReplicaSet?
- ReplicaSet vs ReplicationController
- ReplicaSet vs Bare Pods

ReplicaSet vs ReplicationController
- ReplicaSet is enhanced version of ReplicationController
- ReplicaSet support for [set-based & equality-based](https://kubernetes.io/docs/concepts/overview/working-with-objects/labels/) selectors but ReplicationController not support
- ReplicaSet generally used with deployments
- ReplicaSet
```bash
spec:
   replicas: 3
   selector:
   matchExpression:
      - {key: app, operator: In, values: [example, example, rs]}
      - {key: tier, operator: NotIn, values: [production]}
   template:
      metadata:
```
- ReplicationController
```bash
spec:
   replicas: 3
   selector:
      app: alpine-app
   template:
      metadata:
```

| Bare Pod                                | ReplicaSet                                                    |
| :-------------------------------------- | :------------------------------------------------------------ |
| Its not have labels                     | Its have labels                                               |
| Suitable for simple, non-critical tasks | Suitable for production workloads requiring high availability |
| not self-healing, and not scalability   | self-healing, and scalability                                 |
| No Scaling                              | Scaling                                                       |

[Deployment:](https://kubernetes.io/docs/concepts/workloads/controllers/deployment/) Its one level higher/desire abstraction of ReplicaSet.
deployment can control 2 components such as 
1. Pod & 
2. ReplicaSet

Use case
- Create Pod
- Update Pod
- Rolling Upgrade Pod
- Rolling Downgrade Pod
- Pause/Resume Pod

```bash
kubectl apply -f deployment.yaml
kubectl get deployment nginx-deployment
kubectl rollout status deployment deployment
kubectl describe deployment
kubectl rollout history deployment/nginx-deployment
kubectl rollout status deployment
kubectl describe deployment
kubectl delete -f deployment.yaml
```