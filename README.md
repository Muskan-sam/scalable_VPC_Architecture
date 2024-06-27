# Scalable Virtual Network Deployment with AWS VPC - DevOps project

![AWS-Cloud](https://imgur.com/AXD50yl.png)

## Problem Statement

Deploying a modular and scalable virtual network architecture on AWS can be complex, involving multiple steps and configurations. The goal is to set up a secure, highly available, and auto-scalable infrastructure using Amazon VPC, ensuring efficient network management and seamless deployment of application servers.

## Solution

### 1. Pre-Requisites
- Ensure you have an AWS account to create infrastructure resources on AWS Cloud.

### 2. Source Code
- Customize the application dependencies on an AWS EC2 instance and create a Golden AMI with the following installations:
  - AWS CLI
  - Apache Web Server
  - Git
  - CloudWatch Agent
  - Push custom memory metrics to CloudWatch
  - AWS SSM Agent

### 3. VPC Deployment
- Build a VPC network (`192.168.0.0/16`) for Bastion Host deployment.
- Build a VPC network (`172.32.0.0/16`) for deploying highly available and auto-scalable application servers.
- Create a NAT Gateway in the Public Subnet and update the Private Subnet's route table for outbound internet connection.
- Create a Transit Gateway and associate both VPCs for private communication.
- Create Internet Gateways for each VPC and update Public Subnet's route table for inbound/outbound internet connection.
- Create a CloudWatch Log Group with two Log Streams to store VPC Flow Logs.
- Enable Flow Logs for both VPCs and push them to CloudWatch Log Groups.

### 4. Instance and Service Setup
- Create a Security Group for the bastion host allowing port 22 from the public.
- Deploy a Bastion Host EC2 instance in the Public Subnet with an Elastic IP (EIP) associated.
- Create an S3 Bucket to store application-specific configurations.
- Create a Launch Configuration with the following:
  - Golden AMI
  - Instance Type: `t2.micro`
  - User data to pull code from Bitbucket Repository and start the HTTPD service.
  - IAM Role granting access to Session Manager and the S3 bucket (without full S3 access).
  - Security Group allowing port 22 from Bastion Host and port 80 from Public.
  - Key Pair

### 5. Auto Scaling and Load Balancing
- Create an Auto Scaling Group with Min: 2, Max: 4 across two Private Subnets in different availability zones (1a and 1b).
- Create a Target Group and associate it with the Auto Scaling Group.
- Create a Network Load Balancer in the Public Subnet and add the Target Group.
- Update the Route53 hosted zone with a CNAME record routing traffic to the Network Load Balancer.

### 6. Validation
- As a DevOps Engineer, log in to Private Instances via the Bastion Host.
- Use AWS Session Manager to access the EC2 shell from the console.
- Browse the web application using the domain name from a public internet browser and verify that the page loads correctly.

## Output

By following the above steps, the solution achieves the following outcomes:
- **Modular and Scalable Architecture:** Deployed using Amazon VPC, ensuring secure and efficient network management.
- **High Availability:** Configured auto-scaling and load balancing, ensuring the application can handle traffic spikes and maintain uptime.
- **Enhanced Monitoring:** Implemented CloudWatch for system performance monitoring and custom memory metrics.
- **Secure Access:** Utilized a Bastion Host and AWS Session Manager for secure access to private instances.
- **Efficient Deployment:** Automated setup using AWS CLI, Golden AMI, and Launch Configurations, streamlining the deployment process.
- **Validation:** Verified application accessibility and functionality through a public domain name, ensuring the deployment is successful and operational.

This structured approach ensures that the infrastructure is robust, scalable, and capable of handling production workloads effectively.
