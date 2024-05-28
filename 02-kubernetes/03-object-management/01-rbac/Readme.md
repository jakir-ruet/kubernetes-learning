#### Lets get started
##### Add user named Jasim in K8s Cluster
Create namespace named 'development'
```bash
kubectl create namespace development
```
Create private key & a Certificate Signing Request (CSR) for Jasim user
```bash
cd ${HOME}/.kube
sudo apt-get install openssl
sudo openssl genrsa -out Jasim.key 2048
sudo openssl req -new -key Jasim.key -out Jasim.csr -subj "/CN=Jasim/O=development"
# Where CN > Common Name & O > Organization
```
Provide CA keys of K8s cluster to generate the certificate
```bash
sudo openssl x509 -req -in Jasim.csr -CA ${HOME}/.minikube/ca.crt -CAkey ${HOME}/.minikube/ca.key -CAcreateserial -out Jasim.crt -days 45
```
View & add the user in the Kubeconfig file.
```bash
kubectl config view
kubectl config set-credentials Jasim --client-certificate ${HOME}/.kube/Jasim.crt --client-key ${HOME}/.kube/Jasim.key
kubectl config view
```
Add a context in the config file, that will allow this user (Jasim) to access the development namespace in the cluster.
```bash
kubectl config set-context Jasim-context --cluster=minikube --namespace=development --user=Jasim
```

##### Create a role for user Jasim
Test access by attempting to list pods
```bash
kubectl get pods --context=Jasim-context 
```
Create a role resource using manifest & create role & verify role
```bash
sudo touch pod-reader-role.yaml
kubectl apply -f pod-reader-role.yaml
kubectl get role -n development
```

##### Bind the role to the Jasim & verify your setup works
Create role binding, test access by attempting to list pods & create pod
```bash
sudo touch pod-reader-role-binding.yaml
kubectl apply -f pod-reader-role-binding.yaml
kubectl get pods --context=Jasim-context
kubectl run nginx --image=nginx --context=Jasim-context
```