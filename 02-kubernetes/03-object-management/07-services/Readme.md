In Kubernetes, a [Service](https://kubernetes.io/docs/concepts/services-networking/service/) is a method for exposing a network application that is running as one or more Pods in your cluster.
- its used to access the pods from outer world
- its abstract layer between pods & client
- its provides a way to expose application as a set of pods.

***Service Routing***
- client make request to service, which route traffic to pods in ***load balancer*** manner.

***EndPoints***
Each Service has a Type. ServiceType define how and where Service will Expose the Application.
- its backend entities, to which service route traffic.
- each pod have endpoint associate with service
```bash
kubectl get endpoints serviceName
```

***Service Types***
- [ClusterIP](https://kubernetes.io/docs/concepts/services-networking/service/#type-clusterip)
  - ClusterIP Service expose Application within Cluster Network.
  - Use ClusterIP, when client is Other Pods within the Cluster.
    how the client can access to pod via service,
    requirements
    1. deployment (pod)
    2. clusterip-service
    3. tmp-pod (due to service only accessible in k8s network)
    ```bash
    kubectl apply -f svc-pod.yaml
    kubectl describe deployment nginx-server
    kubectl get pods -o wide # available 3 pods
    kubectl get pod -o wide --show-labels
    kubectl apply -f clusterip-svc.yaml
    kubectl describe service nginx-service
    curl nginx-service:8080 # getting an error, due its only within k8s network, need temp pod
    kubectl apply -f svc-test-pod.yaml
    kubectl exec svc-test-pod -- curl nginx-service:8080 # see nginx default page
    ```
- [NodePort](https://kubernetes.io/docs/concepts/services-networking/service/#type-nodeport)
  - NodePort Service expose Application Outside Cluster Network.
  - Use NodePort, when client is accessing the Service from Outside the Cluster.
  ```bash
  kubectl apply -f nodeport-service.yaml
  kubectl describe nginx-service-nodeport
  curl localhost:35005
  # after allow this port 35005 in AWS/GCP in inbound policy
  http://IPofInstance:35005
  ```

- [LoadBalancer](https://kubernetes.io/docs/concepts/services-networking/service/#loadbalancer)
  - Load Balancer Service also expose Application to Outer World but Cloud LB is required.
- [ExternalName](https://kubernetes.io/docs/concepts/services-networking/service/#externalname)

***Service DNS***
- Kubernetes DNS assign DNSNames to Services, allow applications within Cluster to easily locate the Service.
- Service Fully Qualified Name has the following format- Service-name.namespace-name.svc.cluster-domain.example
- Defauly Cluster Domain is cluster.local

***Service DNS & Namespaces***
- Service fully qualified Domain Name can be used to reach service from within any Namespace in Cluster. `service-name.namespace-name.svc.cluster.local`
- Pods within the same NameSpace can use the Service Name Only.
`service-name`

[Ingress]()
- Manage the External Access to Service.
- Apart from NodePort Service, Ingress is capable of many more.
- Provide the SSL Termination, Load Balancing, NameBase Virtual Hosting.

[Ingress Controllers](https://kubernetes.io/docs/concepts/services-networking/ingress-controllers/)
- In order for the Ingress resource to work, the cluster must have an ingress controller running.
- Variety of Ingress Controller available in K8s to provide the multiple mechanism for external access of Service.
- You can deploy any number of Ingress Controller.
- Ingress define a set of Routing Rules.
- Each Rule has a set of Paths, each with a Backend. Request matching a path will be routed to associated Backend.
- If Service Use ***NamedPort***, ingress can also use the port’s name to choose to which port it will route.