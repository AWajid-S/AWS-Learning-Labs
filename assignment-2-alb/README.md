# Assignment 2 — Application Load Balancer (ALB)

## Objective
Deploy two EC2 instances behind an Application Load Balancer (ALB).
The ALB must handle all incoming traffic. EC2 instances must not be directly accessible from the internet.

## Architecture Summary
Internet → ALB (public subnets) → Target Group → EC2 instances

## Tasks 

### 1. EC2 Instances
- Two EC2 instances launched in the same VPC
- Placed in different Availability Zones
- Web server installed using user-data
- Each instance returns different content

### 2. Application Load Balancer
- ALB created in two public subnets
- HTTP listener on port 80
- Target group created
- Both instances registered
- Health check configured on `/`

### 3. Security Groups
- ALB security group allows HTTP from anywhere
- EC2 security group allows HTTP only from the ALB security group
- EC2 instances are not directly accessible from the internet

### 4. Testing
- Accessed ALB DNS name
- Page refresh shows traffic switching between instances
- Target group health checks confirmed healthy

## Bonus
- Route 53 ALIAS record pointing to ALB
- HTTPS listener using ACM
- Auto Scaling Group attached to ALB

## Evidence
Screenshots stored in `/screenshots`
Notes stored in `/notes`
