#### Requirement of complete deployment
1. Deployments/Pods [2]
   - MongoDB (Private Interaction)
   - MongoExpress (Public Interaction)
2. Services [2]
   - Internal Service
   - External Service
3. ConfigMap (Database URL) [1]
4. Secret (DB UserName & Password) [1]

git config --global init.defaultBranch master