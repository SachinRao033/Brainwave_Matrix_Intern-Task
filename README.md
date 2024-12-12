# Deploy-the-Web-Application-in-AWS & Kubernetes

"Task 1(Video link): https://drive.google.com/file/d/1sn4aBdfJtUpfDtsYAoxPHQaixZG5vT-l/view?usp=sharing "

DEPLOY A WEB APPLICATION IN AWS/KUBERNETES

Step 1:- Launch a New EC2 Instance (Amazon Linux - t2.micro)   

•	Go to the AWS Management Console and launch a new EC2 instance.   
•	Choose "Amazon Linux 2 AMI" as the operating system.  
•	Select an instance type (t2.micro is a good choice for testing purposes and free tier).  
•	Create new key pair and download it.Configure security groups, allowing SSH (port 22) and HTTP (port 80).  
•	Launch the instance and connect to it via SSH (In MobaXterm Tool).  

![EC2 Instance Image](https://github.com/user-attachments/assets/d5594343-6000-4423-a2b6-f03f0a709b6d)

Step 2:- Install kubectl (Kubernetes CLI)

•	After connecting to the EC2 instance, run the following commands to install kubectl:  
   --"curl -O https://s3.us-west-2.amazonaws.com/amazon-eks/1.30.0/2024-05-12/bin/linux/amd64/kubectl  
      sudo mv ./kubectl /usr/local/bin   
      chmod +x ./kubectl  
      kubectl version"   
•	This will download kubectl, move it to /usr/local/bin for execution, apply the necessary permissions, and verify the version.  

 ![Install kubectl Image](https://github.com/user-attachments/assets/7603c1fa-5a53-4832-badb-85f5444f0a51)

Step 3:- Install AWS CLI

•	Install the AWS Command Line Interface (CLI) using these commands:   
   --"curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"  
      unzip awscliv2.zip  
      sudo ./aws/install  
      aws --version"  
•	This installs the latest version of the AWS CLI and verifies the installation.

 ![Install AWS CLI Image](https://github.com/user-attachments/assets/04eb062f-fcd5-4d3f-8269-ec454325fa3b)

Step 4:- Install eksctl (EKS Cluster Management Tool)

•	To simplify EKS cluster creation, install eksctl:   
   --"curl --silent --location "https://github.com/weaveworks/eksctl/releases/latest/download/eksctl_$(uname -s)_amd64.tar.gz" | tar xz -C /tmp    
      sudo mv /tmp/eksctl /usr/local/bin    
      eksctl version"    
•	This command downloads and installs eksctl, a command-line tool for managing EKS clusters.
 
![Install eksctl Image](https://github.com/user-attachments/assets/64409869-2ded-48c4-ae03-1ac9f7a29657)

Step 5:- Create a New IAM Role
•	Go to the IAM section in the AWS Console and create a new role with the following permissions:  
  o	IAM FullAccess – We need create all these roles individually (or) otherwise we give one role is “AdministratorAccess” (VPC FullAccess,EC2    FullAccess,CloudFormation FullAccess).   
 
![IAM Role Image](https://github.com/user-attachments/assets/af5371ee-5af4-496e-a779-7f1c1f4a3e8b)

Step 6:- Attach IAM Role to the EC2 Instance  
•	Attach the created role to the EC2 instance used as the management host. This allows the instance to interact with other AWS services.  

![Attach IAM role to EC2 Instance](https://github.com/user-attachments/assets/db383e7a-1100-4183-adbc-8bba42a9d24f)



Step 7:- Create EKS Cluster using eksctl 

•	To create the EKS cluster, use the eksctl command.  
   --Mumbai (ap-south-1):
    "eksctl create cluster --name demo-cluster --region ap-south-1 --node-type t2.micro --zones ap-south-1a,ap-south-1b "

•	This will create a new EKS cluster in the specified region, using the specified instance type and zones.
 

 

Step 8:- Deploy Nginx Pods on Kubernetes

•	Create a Deployment for Nginx: To deploy an given demo-web-httpd application with 1 replicas:
kubectl create deployment demo-web-httpd  --image=ss1927/httpd          --replicas=1   --port=80

•	Check the Status:
List all resources - kubectl get all
List the running pods - kubectl get pods

•	After all this,To run the webapp with help this command:
Kubectl run webapp –image=ss1927/httpd
 

Step 9:- Expose the Deployment as a Service

•	To expose the Nginx deployment via a LoadBalancer, follow these steps:
Expose the Deployment: kubectl expose deployment demo--web-httpd    --port=80 --type=LoadBalancer

Check the Service: To see the service details and external IP (LoadBalancer IP)
kubectl get services -o wide

 
 

 

Step 10:-Output -- Select Auto Scaling Group.
•	Open that selected auto scaling group and after it shows details of auto scaling.
•	After click to copy the DNS name and go to any browser, just put the link to search.
 

 

 

