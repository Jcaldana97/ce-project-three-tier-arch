# Three-tier Architecture Project

## Project overview

The purpose of the project is the design, building and deployment of a complete and secure production-ready 3-tier application architecture using the Cloud Core and Networking Services.

## Architecture description

- VPC with Networking Segmentation

- 3-tier architecture 
	- Presentation 
	- Application
	- Data
	
- Application tier load-balanced
	- Load Balancer implemented with health checks.

- Highly available application
	- NAT Gateway available in multiple availability zones. 
	
- Secure networking with proper isolation
	- ALB SG: Allow 80/443 from 0.0.0.0/0
	- App SG: Allow 8080/443 from ALB SG only
	- Data SG: Allow 3306/5432 from App SG only
	- Bastion SG: Allow SSH from local IP to the other SGs (other SGs must include SSH inbound rule form this SG, just for debugging purpose)


## How to deploy/replicate

1. Network CIDR Planning 
	- VPC: 10.0.0.0/16
	- Presentation Subnets (Public): 
		- AZ 1: 10.0.1.0/24
		- AZ 2: 10.0.2.0/24
	-Application Subnets (Private):
		- AZ 1: 10.0.11.0/24
		- AZ 2: 10.0.12.0/24
	- Data Subnets (Private): 
		- AZ 1: 10.0.21.0/24
		- AZ 2: 10.0.22.0/24
		
2. Creation of a VPC
	- Specify CIDR Block (10.0.0.0/16)
	- Enable DNS Hostnames

3. Creation Internet Gateway
	- Attachment of IGW to the created VPC
	
4. Creation of Public and Private Subnets
	- Create Presentation (Public) Subnets (CIDR Blocks: 10.0.1.0/24, 10.0.2.0/24) enabling auto-assign public IP
	- Create Application (Private) Subnets (CIDR Blocks: 10.0.11.0/24, 10.0.12.0/24)
	- Create Data (Private) Subnets (CIDR Blocks: 10.0.21.0/24, 10.0.22.0/24)

5. Creation of Route Tables 
	- Create Public Route Table for the created VPC
	- Add route to Internet Gateway with destination CIDR Block to anywhere (0.0.0.0/0)
	- Associate Presentation Subnets to the Public Route Table. 
	- Create Private Route Table for the created VPC
	- Associate Application and Data Subnets to the Private Route Table. 

6. Configurion of NAT Gateway
	- Allocate an Elastic IP with vpc domain, which will be used for the NAT Gateway
	- Deploy NAT Gateway in one Public Subnet
	- Update Private Route Table to attach NAT Gateway

7. Create Security Groups
	- ALB, App, Data and Bastion SGs
	
8. Launch EC2 Instances
	- Create Instances for App Tier (2 for each subnet, in different availability zones)
	- Instances must be in the application subnet with the App SG attached

9. Creation of Target Group
	- Create a target group with the following details: 
		- VPC ID of the current VPC
		- Health-check protocol: HTTP
		- Health-check port: 80
		- Health-check path: /health
		- Health-check interval: 10 seconds
		- Health-check timeout: 5 seconds
		- Healthy threshold count: 2 
		- Unhealthy threshold count: 2

10. Creation of Load Balancer 
	- Create Load Balancer available for both Public Subnets with the dedicated Security Group attached (ALB SG) with an Internet-facing scheme. 
	- Create a Listener to the Load Balancer in port 80 protocol HTTP with the Target Group created attached.

11. Cost allocation tags
	- Tag costly resources per:
		- Project
		- Resource Type
	- Activate Tags in the Cost Explorer. 
	
12. Set CloudWatch alarms
	- Create a SNS subscription to receive notifications to e-mail recipient
	- Create alarms for app-tier instances monitoring High CPU Utilization
	- Create alarms for app-tier instances to be triggered when instance status check fails

13. Activate VPC Flow Logs
	- Create a Log Group in CloudWatch to store the flow logs of the VPC
	- Create a Policy that allow the creation of log groups, log stream, put log events, describe log groups and log streams. 
	- Create an IAM Role for VPC Flow logs and attach the policy created
	- Create flow logs for the VPC of the project using the IAM role and log groups created.

## Testing instructions

## Cost breakdown



