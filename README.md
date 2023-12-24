[![Youtube][youtube-shield]][youtube-url]
[![Facebook-Page][facebook-shield]][facebook-url]
[![LinkedIn][linkedin-shield]][linkedin-url]

<h3 align="center">
   Visit Us <a href="http://www.lapissoft.com">Lapis Soft</a>
</h3>

### Docker

Docker is a platform and set of tools designed to facilitate the creation, deployment, and running of applications in lightweight, portable containers. Containers allow developers to package an application and its dependencies, including libraries and other components, into a single, standardized unit. This unit can then be easily moved between different environments, such as development, testing, and production, without worrying about differences in the underlying infrastructure.

### Kubernetes

Kubernetes, often abbreviated as K8s, is an open-source container orchestration platform that automates the deployment, scaling, and management of containerized applications. It was originally developed by Google and is now maintained by the Cloud Native Computing Foundation (CNCF). Kubernetes provides a powerful and flexible platform for container orchestration, allowing you to deploy and manage applications seamlessly across a cluster of machines.

#### Docker Installation

- <a href="https://docs.docker.com/get-docker/">How to Install?</a>

#### How to pull from Docker Hub

- `docker pull jakirbd/doc-kub-first-app:latest`

#### Run the downloaded docker image & access to the application

- `docker run --name AppName -p 3000:80 -d UserName/AppName:TagName OutputUrl`
- `docker run --name doc-kub-first-app -p 3000:80 -d jakirbd/doc-kub-first-app:latest`

#### Copy the docker image name from Docker hub

- `docker run --name doc-kub-first-app -p 3000:80 -d jakirbd/doc-kub-first-app:latest`

#### Essential Commands of Docker

| SL  | Command            | Functionality                                       |
| :-: | :----------------- | :-------------------------------------------------- |
|  1  | `docker --version` | It is used to key-value pair (mapping) presentation |
|  2  | `docker login`     | We can access using credential                      |
|  3  | `docker ps`        | Show only running container                         |
|  4  | `docker ps -a`     | Show all container                                  |
|  5  | `docker ps -a -q`  | Show all container with id                          |
|  6  | `docker images`    | Show all images                                     |

#### CLIs in Docker-Kubernetes

1. AWS CLI

   - Control multiple AWS services from this command line.
   - <a href="https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html">How to Install?</a>
   - Let's me check `aws --version`
   - If its okay then we will see `aws-cli/2.15.4 Python/3.11.6 Darwin/23.2.0 exe/x86_64 prompt/off`
   - Configuration using security credential
     - Go to AWS Management Console > Services > IAM
     - Select the IAM User Name: You User Name [***NB***: You must use IAM's Information only not Root User]
     - Click on `Security credentials`
     - Click on `Create access key`
     - Copy Access ID & Secret access key
     - Go to your Terminal and implement as below format
     - `aws configure`
     - AWS Access Key ID [None]: Put your ID here and press Enter.
     - AWS Secret Access Key [None]: Put your secret key here and press Enter
     - Default region name [None]: us-east-1
     - Default output format [None]: json
   - Let's me check whether the configuration is done.
     - `aws ec2 describe-vpcs`
     - If it is done then we will see the details of the default vpc.

2. kubectl

   - Control the kubernetes clusters & objects.
   - <a href="https://docs.aws.amazon.com/eks/latest/userguide/install-kubectl.html">How to Install?</a>
   - `mkdir kubectlbinary`
   - `cd kubectlbinary`
   - `curl -O https://s3.us-west-2.amazonaws.com/amazon-eks/1.28.3/2023-11-14/bin/darwin/amd64/kubectl`
   - Assign the exexute permissions `chmod +x ./kubectl`
   - Set the path by copying to user home directory `mkdir -p $HOME/bin && cp ./kubectl $HOME/bin/kubectl && export PATH=$HOME/bin:$PATH` & `echo 'export PATH=$HOME/bin:$PATH' >> ~/.bash_profile`

- Let's me check whether the configuration is done. `kubectl version --client`
- If it shows the following output then installation is done.
  - `Client Version: v1.28.2`
  - `Kustomize Version: v5.0.4-0.20230601165947-6ce0bf390ce3`

3. eksctl
   - creating-deleting clusters on AWS EKS.
   - create, autoscale & delete the node groups.
   - create fargate profiles.
   - it is powerfull tool for managing EKS clusters on AWS.
   - <a href="https://docs.aws.amazon.com/emr/latest/EMR-on-EKS-DevelopmentGuide/setting-up-eksctl.html">How to Install?</a>
   - If you do not already have Homebrew installed on macOS, install it with the following command. `/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install.sh)"`
   - Install the Weaveworks Homebrew tap. `brew tap weaveworks/tap` or `brew install weaveworks/tap/eksctl`
   - Test that your installation was successful with the following command. You must have eksctl 0.34.0 version or later. `eksctl version`
   - If it shows the following output then installation is done.
   - `0.167.0`

## Courtesy of Jakir,

<a href="https://www.linkedin.com/in/jakir-ruet/">LinkedIn</a>
<a href="https://www.facebook.com/jakir.ruet">Facebook</a>
<a href="https://github.com/jakir-ruet">GitHub</a>
<a href="https://web.skype.com/?openPstnPage=true">Skype</a>

## Have a good day, stay with me.

[youtube-shield]: https://img.shields.io/badge/-Youtube-black.svg?style=flat-square&logo=youtube&color=blue&logoColor=red
[youtube-url]: https://www.youtube.com/@LapisSoft/featured
[facebook-shield]: https://img.shields.io/badge/-Facebook-black.svg?style=flat-square&logo=facebook&color=pink&logoColor=blue
[facebook-url]: https://www.facebook.com/GoLapisSoft/
[linkedin-shield]: https://img.shields.io/badge/-LinkedIn-black.svg?style=flat-square&logo=linkedin&colorB=red
[linkedin-url]: https://www.linkedin.com/company/lapis-soft/
