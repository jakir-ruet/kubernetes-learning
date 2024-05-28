#### Welcome to Object Management
Necessary commands
- Kubectl get
- Kubectl describe
- Kubectl create
- Kubectl apply
- Kubectl delete
- Kubectl exec

K8s RBAC Management
- Object
  - ***Roles*** (Define permission only within namespace in cluster)
    After assign the permission ***RoleBinding*** to the specific user 😐.
  - ***ClusterRoles*** (Define permission across the cluster. Not limited to specific namespace)
    After assign the permission ***ClusterRoleBinding*** to the specific user 😐.