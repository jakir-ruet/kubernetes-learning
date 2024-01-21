#### Requirement of complete deployment
1. Deployments/Pods [2]
   - MongoDB (Private Interaction)
   - MongoExpress (Public Interaction)
2. Services [2]
   - Internal Service
   - External Service
3. ConfigMap (Database URL) [1]
4. Secret (DB UserName & Password) [1]

#### Find the base64 value of username & password
```
   echo -n 'username' | base64
```

```
   echo -n 'password' | base64
```

#### After configuring the mongo-deployment.yaml & mongo-secret.yaml
- deploying the mongo-deployment.yaml by following command [we must run the minikube ```minikube start```]
```
   kubectl apply -f mongo-deployment.yaml
```
After it will make a pod named "mongo-deployment"
```
   kubectl get pod
```
See in details of the pod
```
   kubectl describe pod PodName
```

#### type service code in mongo-deployment.yaml file
```
   apiVersion: v1
   kind: Service
   metadata: 
   name: mongodb-service
   spec:
   selector:
      app: mongodb
   ports:
      - protocol: TCP
      port: 27017
      targetPort: 27017
```
Reapply & check service
```
   kubectl apply -f mongo-deployment.yaml
```
```
   kubectl get service
```
