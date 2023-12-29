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

### Types of Processes

There are fundamentally two types of processes in Unix based OS:

#### Foreground processes

These are initialized and controlled through a terminal session (referred to as interactive processes). In other words, there has to be a user connected to the system to start such processes; they haven’t started automatically as part of the system functions/services.

#### Background processes

Are processes not connected to a terminal (referred to as non-interactive/automatic processes); they don’t expect any user input.

### Attached-Detached

We can run container in attached mode (in the foreground) or in detached mode (in the background). By default, Docker runs the container in attached mode. In the attached mode, Docker can start the process in the container and attach the console to the process's standard input, standard output, and standard error.

#### Docker Installation

- <a href="https://docs.docker.com/get-docker/">How to Install?</a>

#### How to pull from Docker Hub

- `docker pull jakirbd/doc-kub-first-app:latest`

#### Run the downloaded docker image & access to the application

- `docker run --name AppName -p 3000:80 -d UserName/AppName:TagName`
- `docker run --name doc-kub-first-app -p 3000:80 -d jakirbd/doc-kub-first-app:latest`
- `docker exec -it AppName /bin/sh`
- `docker exec -it doc-kub-first-app /bin/sh`

#### Essential Commands of Docker

| SL  | Command                 | Explanation                    |
| :-: | :---------------------- | :----------------------------- |
|  1  | `docker --version (-v)` | Checking the version of docker |
|  2  | `docker login`          | We can access using credential |
|  3  | `docker logout`         | We can logout                  |

#### Essential command of images

| SL  | Command                                          | Explanation                                                 |
| :-: | :----------------------------------------------- | :---------------------------------------------------------- |
|  1  | `docker image`                                   | Show the command details                                    |
|  2  | `docker images or docker ls`                     | Show image list                                             |
|  3  | `docker pull ImageName`                          | Pull/Download the image                                     |
|  4  | `docker pull ImageName:TagName`                  | Pull/Download the image with tag name                       |
|  5  | `docker run ImageName (node/nginx)`              | Will be Run & Publish a new container for each publish      |
|  6  | `docker run -it ImageName (node/nginx)`          | Enter into the interactive mode                             |
|  7  | `docker build -t doc-kub-first-app:latest .`     | Build the images with tag (name/version/others) (own image) |
|  8  | `docker image tag ImgId UserName/ImgName:latest` | Image renaming/taging (own image)                           |
|  9  | `docker push jakirbd/my-node-server`             | Pushing the image (own image)                               |
| 10  | `docker image history ImageId`                   | History of image                                            |
| 11  | `docker image inspect ImageId`                   | Inspections the image                                       |
| 12  | `docker image prune -a`                          | Remove all unused images, not just dangling ones            |
| 13  | `docker rmi ImageId`                             | Image remove                                                |

#### Essential command of container

| SL  | Command                                                               | Explanation                                            |
| :-: | :-------------------------------------------------------------------- | :----------------------------------------------------- |
|  1  | `docker container`                                                    | Show the command details                               |
|  2  | `docker container ls`                                                 | Show the enlisted container                            |
|  3  | `docker ps`                                                           | Show only running container                            |
|  4  | `docker ps -a`                                                        | Show all container                                     |
|  5  | `docker ps -a -q`                                                     | Show all container with id (quiet)                     |
|  6  | `docker build .`                                                      | Build a container                                      |
|  7  | `docker build -t TagName .`                                           | Build a container with tag                             |
|  8  | `docker run -p 3000:80 nginx/node/https`                              | Will be Run & Publish a new container for each publish |
|  7  | `docker run -p 3000:80 BaseImageId`                                   | Will be Run & Publish a new container for each publish |
|  9  | `docker rename OldContName NewContName`                               | Renaming the container                                 |
| 10  | `docker run -p 3000:80 -d --name NewContName OldContName`             | Renaming & publishing container                        |
| 11  | `docker run -p 3000:80 -d --rm --name NewContName OldContName`        | Renaming, removing & publishing container              |
| 12  | `docker run -p 3000:80 -d --rm --name NewContName OldContName:latest` | Renaming, removing & publishing using tag container    |
| 13  | `docker run -p 3000:80 -d BaseImageId`                                | Publish the container as detach                        |
| 14  | `docker run -p 3000:80 -d --rm BaseImageId`                           | Container is Remove after stop the container           |
| 15  | `docker exec -it ContainerName /bin/sh`                               | Container connect to terminal using shell              |
| 16  | `docker exec -it ContainerName /bin/bash`                             | Container connect to terminal using bash               |
| 17  | `docker exec -it ContainerName /bash`                                 | Container connect to terminal using bash               |
| 18  | `docker cp index.html my-nginx-server:/usr/share/nginx/html`          | Moving the source file local pc to docker nginx server |
| 19  | `docker container prune`                                              | Remove all container                                   |
| 20  | `docker start ContainerName`                                          | Container start                                        |
| 21  | `docker stop ContainerName`                                           | Container stop                                         |
| 22  | `docker restart ContainerName`                                        | Container restart                                      |
| 23  | `docker rm ContainerName`                                             | Container remove after stop it                         |
| 24  | `docker attach ContainerName`                                         | Attach the container                                   |
| 25  | `docker logs ContainerName`                                           | See the logs details                                   |
| 26  | `docker logs -f ContainerName`                                        | See the future logs details                            |

#### Data-Storage

##### Data

- Application Data (Code, dependencies, package.json Environment)

  - written by developer
  - added to image & container in build phase.
  - read-only/fixed once image is build

- Temporary App Data (Generated data, Enter user input into form)

  - fetched/produced in running container
  - stored in memory or temporary files
  - read + write possible temporary stored in containers

- Permanent App Data (User accounts)

  - fetched/produced in running container
  - stored in files or a database, must not lost container stop/restart
  - read + write possible permanent containers & volumes

##### Storage

- Volumes (managed by docker)
  - Anonymous Volumes
  - Named Volumes
- Bind/Host Mounts (managed by we)
- <a href="https://docs.docker.com/storage/">Manage data in Docker</a>

#### Essential command of volume

| SL  | Command                                                                  | Explanation                                                  |
| :-: | :----------------------------------------------------------------------- | :----------------------------------------------------------- |
|  1  | `docker volume create`                                                   | Create a anonymous volume                                    |
|  2  | `docker volume create my-sweet-vol`                                      | Create a volume                                              |
|  3  | `docker volume ls`                                                       | Check the volume list                                        |
|  4  | `docker volume inspect VolName`                                          | Inspect the volume                                           |
|  5  | `docker volume rm VolName`                                               | Remove the volume                                            |
|  6  | `docker volume prune`                                                    | Remove the anonymous volume                                  |
|  7  | `docker build -t ImgName(OldContName):volumes .`                         | Create own images tag named volumes                          |
|  8  | `docker run -d -p 3000:80 --rm --name NewContName OldContName:volumes`   | Create own images tag named based on volumes                 |
|  9  | `docker rmi ConName:volumes`                                             | Remove the named volume                                      |
| 10  | `docker run -it --name ConName -v /DirName nginx /bin/bash`              | Create a container & anonymous volume mounted on a directory |
| 11  | `docker run -it --name ConName -v VolName:/DirName nginx /bin/bash`      | Create a container & named volume mounted on a directory     |
| 12  | `mkdir /opt/HostDir`                                                     | Create host directory use as volume for app                  |
| 13  | `docker run -it --name ConName -v /opt/HostDir:/HostDir nginx /bin/bash` | Create a image, container on host directory                  |

#### Using Docker Volumes for Jenkins | Named Volumes and Bind Volumes

#### CLIs in Docker-Kubernetes

#### Footnote about volume

- Storage persistent location outside of container.
- If container removed then volume will be available on storage.
- It use for the data security purpose.

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
