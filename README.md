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
 
![IAM Role Image](https://github.com/user-attachments/assets/db383e7a-1100-4183-adbc-8bba42a9d24f)

Step 6:- Attach IAM Role to the EC2 Instance 

•	Attach the created role to the EC2 instance used as the management host. This allows the instance to interact with other AWS services.  

![Attach IAM role to EC2 Instance Image](https://github.com/user-attachments/assets/a4475397-b88f-4f03-b33a-44480594e039)

![Attach IAM to EC2 Image](https://github.com/user-attachments/assets/26b5d7ef-65a3-4f3d-8751-a41a6a8fca6d)

Step 7:- Create EKS Cluster using eksctl 

•	To create the EKS cluster, use the eksctl command.  
   --Mumbai (ap-south-1):   
    "eksctl create cluster --name demo-cluster --region ap-south-1 --node-type t2.micro --zones ap-south-1a,ap-south-1b"
•	This will create a new EKS cluster in the specified region, using the specified instance type and zones.
 
![Creating EKS Cluster Image](https://github.com/user-attachments/assets/4bf0bb5c-889b-4874-94e4-7d2b654569f9)

![EKS CLuster Image](https://github.com/user-attachments/assets/54817a6b-3817-4a52-a327-1841928e9f25)

Step 8:- Deploy Web-httpd Pods on Kubernetes

•	Create a Deployment for web-httpd: To deploy an given demo-web-httpd application with 1 replicas:  
    --"kubectl create deployment demo--web-httpd  --image=ss1927/httpd          --replicas=1   --port=80"   
•	Check the Status:  
List all resources - "kubectl get all"   
List the running pods - "kubectl get pods"   
•	After all this,To run the webapp with help this command:    
    --"Kubectl run webapp –image=ss1927/httpd"
    
 ![Deploy Web-httpd Image](https://github.com/user-attachments/assets/8cf6d2c7-2a67-4915-8770-48e33e479b23)

Step 9:- Expose the Deployment as a Service

•	To expose the Nginx deployment via a LoadBalancer, follow these steps:  
    --Expose the Deployment: "kubectl expose deployment demo--web-httpd    --port=80 --type=LoadBalancer"  
    --Check the Service: To see the service details and external IP (LoadBalancer IP)
       "kubectl get services -o wide"

 ![Excuted Load Balancer Code Image](https://github.com/user-attachments/assets/f204a65f-8030-4932-8ea8-087a60283438)

 ![Created Load Balancer Image](https://github.com/user-attachments/assets/9117a461-098d-4d29-a060-a3c5fb11c526)

 ![Creating EC2 Instance help of Auto Scaling Image](https://github.com/user-attachments/assets/fb25c59d-ce92-4f0a-a8d5-4dc403589452)

Step 10:-Output -- Select Auto Scaling Group.  

•	Access the selected Auto Scaling group and review its detailed information, including configurations and instance statuses.   
•	Locate and copy the DNS name associated with the Auto Scaling group. Open a web browser, paste the copied DNS name into the address bar, and press Enter to access the link.    

 ![Created AutoScaling Image](https://github.com/user-attachments/assets/815ecf06-17c4-4342-8555-e0dd5fe3d9c5)

 ![Click to Copy the DNA Name Image](https://github.com/user-attachments/assets/4aeabba5-6ae4-476b-9f7d-0648ff1815c0)

 ![Link to Serach Image](https://github.com/user-attachments/assets/09555aaa-49db-48b5-bd28-fb48b6b245b3)

 ![Click to continue Image](https://github.com/user-attachments/assets/4ae44ec3-e193-495c-81b4-26e35441fdd4)

 ![WEB Application running Image](https://github.com/user-attachments/assets/2cd6f73f-4a27-477c-b8c5-ead74188fea4)

 ![Web Application running Image](https://github.com/user-attachments/assets/73fc6a4e-4682-499c-bd98-c0764e5dd093)

<h3 Align : Center> Thank You </h3>
