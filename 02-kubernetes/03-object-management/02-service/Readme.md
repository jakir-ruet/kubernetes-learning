[service accounts](https://kubernetes.io/docs/concepts/security/service-accounts/)
A service account is a type of non-human account that, in Kubernetes, provides a distinct identity in a Kubernetes cluster.
Why Use
- To authenticate with K8s the container process.
- User need to setup service account to control the access. If Pods needs to communicate with K8s APls.
- Bind service accounts with cluster role or ClusterRoleBinding to provide access to cluster APls.
- Service account access also manage by RBAC.

Check available service
```bash
kubectl get serviceaccounts
kubectl get serviceaccounts -n development # here n is namespace
kubectl get serviceaccounts -n kube-system # here n is namespace
kubectl apply -f my-serviceaccount.yaml
kubectl get serviceaccounts -n development
kubectl get roles -n development
kubectl apply -f pod-reader-binding.yaml
kubectl apply -f service-account-binding.yaml
# role binding to development
kubectl get rolebinding -n development
```