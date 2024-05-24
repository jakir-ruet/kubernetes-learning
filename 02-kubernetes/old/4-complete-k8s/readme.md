#### Complete Kubernetes on Ubuntu

[AWS CLI Install](https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html)

```bash
curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
unzip awscliv2.zip
sudo ./aws/install
/usr/local/bin/aws --version
aws --version
```

[Prerequisite](https://docs.aws.amazon.com/eks/latest/userguide/getting-started-eksctl.html)

[Kubectl Install](https://docs.aws.amazon.com/eks/latest/userguide/install-kubectl.html)

```bash
curl -O https://s3.us-west-2.amazonaws.com/amazon-eks/1.29.0/2024-01-04/bin/linux/amd64/kubectl
chmod +x ./kubectl
mv kubectl /usr/local/bin/
kubectl version
kubectl version --client
```

[Eksctl Install](https://docs.aws.amazon.com/emr/latest/EMR-on-EKS-DevelopmentGuide/setting-up-eksctl.html)

```bash
curl --silent --location "https://github.com/weaveworks/eksctl/releases/latest/download/eksctl_$(uname -s)_amd64.tar.gz" | tar xz -C /tmp
sudo mv /tmp/eksctl /usr/local/bin
eksctl version
```

IAM Role for EKS
- EksRole
  - AmazonEC2FullAccess
  - IAMFullAccess
  - AdministratorAccess
  - AWSCloudFormationFullAccess

Let's move ec2 & role integrate with ec2 instance.

[Create EksCluster](https://docs.aws.amazon.com/eks/latest/userguide/create-cluster.html)

```bash
eksctl create cluster --name my-cluster --region region-code --version 1.28 --vpc-private-subnets subnet-ExampleID1,subnet-ExampleID2 --without-nodegroup
eksctl create cluster --name mycluster --region us-east-2 --node-type t2.small 
```
