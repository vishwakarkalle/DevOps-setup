Project Setup using below tools
Maven
Git Hub
Jenkins
Docker
Kubernetes
Step - 1 : Jenkins Server Setup
Create Ubuntu VM using AWS EC2 (t2.medium)
Enable SSH & 8080 Ports in Ec2 Security Group
Install Java & Jenkins using below commands
$ sudo apt-get update
$ sudo apt-get install default-jdk
$ curl -fsSL https://pkg.jenkins.io/debian-stable/jenkins.io-2023.key | sudo tee
/usr/share/keyrings/jenkins-keyring.asc > /dev/null
$ echo deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc]
https://pkg.jenkins.io/debian-stable binary/ | sudo tee
/etc/apt/sources.list.d/jenkins.list > /dev/null
$ sudo apt-get update
$ sudo apt-get install jenkins
$ sudo systemctl status jenkins
Copy jenkins admin pwd
$ sudo cat /var/lib/jenkins/secrets/initialAdminPassword
Open jenkins server in browser using VM public ip
URL : http://public-ip:8080/
Create Admin Account & Install Required Plugins in Jenkins
Step - 2 : Install Maven & Git in Jenkins
install maven with below command
$ sudo apt install maven -y

install git with below command
$ sudo apt install git -y

Step - 3 : Setup Docker in Jenkins
install docker
$ curl -fsSL get.docker.com | /bin/bash

Add Jenkins user to docker group
$ sudo usermod -aG docker jenkins

Restart Jenkins
$ sudo systemctl restart jenkins

Verify docker installation
$ sudo docker version

Step - 4 : Create EKS Management Host in AWS
Launch new Ubuntu VM using AWS Ec2 ( t2.micro )

Connect to machine and install kubectl using below commands
$ curl -o kubectl https://amazon-eks.s3.us-west-2.amazonaws.com/1.19.6/2021-01-05/bin/linux/amd64/kubectl
$ chmod +x ./kubectl
$ sudo mv ./kubectl /usr/local/bin
$ kubectl version --short --client

Install AWS CLI latest version using below commands

$ sudo apt install unzip
$ cd
$ curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
$ unzip awscliv2.zip
$ sudo ./aws/install
$ aws --version

Install eksctl using below commands
$ curl --silent --location "https://github.com/weaveworks/eksctl/releases/latest/download/eksctl_$(uname -s)_amd64.tar.gz" | tar xz -C /tmp
$ sudo mv /tmp/eksctl /usr/local/bin
$ eksctl version

Step - 5 : Create IAM role & attach to EKS Management Host & Jenkins Server
Create New Role using IAM service ( Select Usecase - ec2 )

Add below permissions for the role

IAM - fullaccess
VPC - fullaccess
EC2 - fullaccess
CloudFomration - fullaccess
Administrator - acces
Enter Role Name (eksroleec2)

Attach created role to EKS Management Host (Select EC2 => Click on Security => Modify IAM Role => attach IAM role we have created)

Attach created role to Jenkins Machine (Select EC2 => Click on Security => Modify IAM Role => attach IAM role we have created)

Step - 6 : Create EKS Cluster using eksctl
Syntax:

eksctl create cluster --name cluster-name
--region region-name
--node-type instance-type
--nodes-min 2
--nodes-max 2 \ --zones ,

Example: $ eksctl create cluster --name ashokit-cluster4 --region us-east-1 --node-type t2.medium --zones us-east-1a,us-east-1b

Note: Cluster creation will take 5 to 10 mins of time (we have to wait). After cluster created we can check nodes using below command.

$ kubectl get nodes

Step - 7 :: Install AWS CLI in JENKINS Server
URL : https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html

Execute below commands to install AWS CLI

$ curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
$ unzip awscliv2.zip
$ sudo ./aws/install
$ aws --version

Step - 8 : Install Kubectl in JENKINS Server
Execute below commands in Jenkins server to install kubectl

$ curl -o kubectl https://amazon-eks.s3.us-west-2.amazonaws.com/1.19.6/2021-01-05/bin/linux/amd64/kubectl
$ chmod +x ./kubectl
$ sudo mv ./kubectl /usr/local/bin
$ kubectl version --short --client

Step - 9 : Update EKS Cluster Config File in Jenkins Server
Execute below command in Eks Management host & copy kube config file data
$ cat .kube/config

Execute below commands in Jenkins Server and paste kube config file
$ cd /var/lib/jenkins
$ sudo mkdir .kube
$ sudo vi .kube/config

check eks nodes
$ kubectl get nodes

Note: We should be able to see EKS cluster nodes here.

Step - 10 : Create Jenkins CI Job
Stage-1 : Clone Git Repo
Stage-2 : Build
Stage-3 : Create Docker Image
Stage-4 : Push Docker Image to Registry
Stage-5 : Trigger CD Job
Step - 11 : Create Jenkins CD Job
Stage-1 : Clone k8s manifestfiles
Stage-2 : Deploy app in k8s eks cluster
Stage-3 : Send confirmatin email
Step - 12 : Trigger Jenkins CI Job
CI Job will execute all the stages and it will trigger CD Job
CD Job will fetch docker image and it will deploy on cluster
Step - 13 : Access Application in Browser
We should be able to access our application
URL : http://Node-public-ip:NodePort/context-path
We are done with our Setup
Step - 14 : After your practise, delete Cluster and other resources we have used in AWS Cloud to avoid billing
