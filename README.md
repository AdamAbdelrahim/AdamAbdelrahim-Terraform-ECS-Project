![Alt text](TerraformECS.png)
---

# Deploying a Dynamic Web Application on AWS with Terraform, Docker, Amazon ECR, and ECS

This repository contains Terraform Modules required to deploy and host a dynamic web application on Amazon Web Services (AWS) using a modern, infrastructure-as-code (IaC) approach. This project leverages a combination of powerful technologies:
 - Terraform: Automates the provisioning and management of AWS resources, ensuring a consistent and reproducible infrastructure.
 - Docker: Packages the web application and its dependencies into a portable container image.
 - Amazon ECR (Elastic Container Registry): Stores and manages the Docker images for secure and reliable deployment.
 - Amazon ECS (Elastic Container Service) orchestrates the deployment and scaling of the containerized application across a cluster of EC2 instances.

## Key Features:

 - Automated Infrastructure: Terraform defines and manages all the necessary AWS resources, including VPCs, subnets, security groups, load balancers, ECS clusters, and more.
 - Containerized Application: Docker encapsulates the web application, making it platform-independent and easy to deploy.
 - Scalable and Reliable: ECS enables dynamic application scaling based on demand, ensuring high availability and performance.
 - Secure Deployment: ECR provides a private registry for storing and managing Docker images, enhancing security.
 - Reproducible Environment: The entire infrastructure and application deployment are codified in Terraform, allowing for easy replication and disaster recovery.
### Benefits:

 - Simplified Deployment: Streamlines deploying complex web applications on AWS.
 - Increased Agility: Enables rapid iteration and updates to the application and infrastructure.
 - Reduced Operational Overhead: Automates infrastructure management, freeing up time and resources.
 - Improved Consistency: Ensures consistent deployments across different environments (development, staging, production).
 - Enhanced Security: Leverages AWS security best practices and containerization to protect the application and data.

#### Architecture Overview

The Dynamic Web Application is deployed on AWS using Terraform, Docker, ECR, and ECS. Through the use of these tools, a rugged, scalable, and secure infrastructure can easily be generated using the Terraform Modules to produce the following:

 - Virtual Private Cloud (VPC): This project utilizes Terraform to automate the deployment of a dynamic web application on AWS.  The application is containerized using Docker and deployed on an ECS cluster within a secure and scalable VPC.  Key features include a 3-tier network architecture, an Application Load Balancer, and an RDS database instance. 
   * This VPC is set up with public and private subnets across two Availability Zones (AZs) for fault tolerance and high availability. It is important to note within this project, the multi_az_deployment variable in the terraform.tfvars file is set to false, meaning  there is only one primary database instance in a single AZ. While this simplifies the deployment, it means that if the AZ where the database resides experiences an outage, the database will become unavailable.  
 - Subnets: Subdivisions of the VPC that allow you to group resources based on their security and access needs. This project utilizes:
   * Public Subnets: For internet-facing resources like the Application Load Balancer (ALB) and NAT Gateways.    
   * Private Subnets: For internal resources like the ECS cluster and RDS database, enhancing security.    
 - Internet Gateway: Enables communication between resources in VPC and the internet.    
 - NAT Gateways: Allow resources in private subnets to access the internet without being directly exposed to it.    

 - Security Groups: Act as virtual firewalls to control inbound and outbound traffic to your resources.    

 - Application Load Balancer (ALB): Distributes incoming web traffic across multiple targets, such as the ECS in this instance running the application, ensuring high availability and fault tolerance. Within this project's ALB, a listener is configured for both the HTTP/HTTPS (80/443) ports while monitored health is performed continuously.

 - Amazon ECS (Elastic Container Service): A fully managed container orchestration service capable of deploying, managing, and scaling containerized applications.

 - ECS Cluster: A logical grouping of ECS container instances that host the application.

 - ECS Task Definitions: Blueprints for this application, defining the Docker image to use, resource requirements, and other configurations (IAM policies/roles), .    

 - ECS Services:  Manage the launch and scaling of tasks based on your defined task definitions.    

 - Amazon ECR (Elastic Container Registry): A fully managed Docker container registry that will store, manage, and deploy Docker images. Within this project, I used a previously stored container in the Amazon Elastic Container Repository.    

 - Amazon RDS (Relational Database Service): A managed relational database service that makes it easy to set up, operate, and scale a relational database in the cloud. Within this project, a snapshot is used as disaster recovery, acting as a backup to restore data in case of unforeseen events.  Snapshots allow for the quick creation of copies for testing and development without affecting the live environment.  Additionally, snapshots enable data migration and provide a historical record for compliance and auditing.  

 - AWS Certificate Manager (ACM):  Handles the complexity of creating and managing SSL/TLS certificates for your application, ensuring secure communication.    

 - Amazon Route 53: A highly available and scalable cloud Domain Name System (DNS) web service. For this project, I recycled the old domain used in my previous project (spaceadoo.com)

## Terraform Files and Their Functions

Here's a brief overview of the Terraform files in this project:

 - acm.tf:  Requests and validates SSL/TLS certificates using AWS Certificate Manager (ACM).    

 - alb.tf: Creates the Application Load Balancer (ALB), target groups, and listeners to distribute incoming traffic.    

 - asg.tf: Sets up the Auto Scaling group to automatically adjust the number of ECS tasks based on CPU utilization.    

 - backend.tf: Configures the Terraform backend to store the state file in an S3 bucket and lock it with DynamoDB.    

 - ecs-role.tf: Defines the IAM roles and policies required for the ECS tasks to interact with other AWS services.    

 - ecs.tf: Creates the ECS cluster, task definitions, and services to deploy and manage the containerized application.

 - nat-gateway.tf: Provisions NAT Gateways in the public subnets to allow resources in private subnets to access the internet.    

 - output.tf: Defines outputs, such as the website URL, that can be accessed after the infrastructure is deployed.    

 - providers.tf: Configures the AWS provider with the region and default tags.    

 - rds.tf: Creates the RDS database instance and associated resources.    

 - route-53.tf:  Creates Route 53 records to map the domain name to the load balancer.    

 - s3.tf: Creates an S3 bucket to store the environment file for the application.    

 - security-groups.tf: Defines the security groups for the different resources, controlling inbound and outbound traffic.    

 - terraform.tfvars: Contains the values for the variables used in the Terraform configuration.

 - variables.tf: Declares the variables used in the Terraform configuration.

 - vpc.tf: Creates the VPC, subnets, internet gateway, and route tables.  

## How to Use

1. Clone this repository to your local machine.
2. Follow the AWS documentation to create the required resources (VPC, subnets, Internet Gateway, etc.) as outlined in the architecture overview.
3. Use the provided scripts to set up the WordPress application on EC2 instances within the VPC.
4. Configure the Auto Scaling Group, Load Balancer, and other services as per the architecture.
5. Access the WordPress website through the Load Balancer's DNS name.

## Contributing

Contributions to this project are welcome! Please fork the repository and submit a pull request with your enhancements.

## AWS Configuration + Website Example
![Alt text](AWSResources1.PNG)
![Alt text](AWSResources2.PNG)
![Alt text](AWSResources3.PNG)
![Alt text](AWSResources4.PNG)
![Alt text](AWSResources5.PNG)
![Alt text](DevelopedWebsite.PNG)


