# Learning Notes – Assignment 2 (Application Load Balancer)

## Overview

This assignment helped me understand how AWS manages incoming traffic in a highly available and secure way rather than sending users directly to EC2 instances.

I built an architecture where all traffic flows through an Application Load Balancer (ALB), allowing AWS to handle load distribution, health checks, and fault tolerance automatically.

---

## What I Built

- Two EC2 instances running web servers  
- Instances deployed across different Availability Zones  
- Application Load Balancer (ALB) in public subnets  
- Target group registered with EC2 instances  
- Health checks configured on `/`  
- Security groups separating ALB and EC2 access  
- EC2 instances not directly accessible from the internet  
- Route 53 ALIAS record pointing to the ALB  
- HTTPS listener using AWS Certificate Manager (ACM)  
- Auto Scaling Group attached behind the ALB  
- Full cleanup of all resources after testing  

---

## Key Concepts I Learned

### 1. EC2 instances should not be public entry points

Even though EC2 instances can have public IP addresses, this is not how production systems are designed.

Instead:
- the ALB is public  
- EC2 instances sit behind the ALB  
- users never connect to EC2 directly  

This significantly improves security and scalability.

---

### 2. The load balancer is the traffic controller

The Application Load Balancer is responsible for:
- receiving all incoming HTTP and HTTPS requests  
- distributing traffic across multiple instances  
- performing health checks  
- removing unhealthy instances from rotation  

EC2 instances themselves do not manage traffic routing.

---

### 3. Target groups act as the backend layer

The ALB does not forward traffic directly to EC2.

Instead, it sends traffic to a target group, which:
- tracks registered instances  
- monitors instance health  
- determines which instances receive traffic  

This abstraction allows instances to be added or removed without affecting users.

---

### 4. Health checks enable self-healing

Health checks are central to fault tolerance.

If an instance fails:
- the ALB marks it as unhealthy  
- traffic is stopped automatically  
- Auto Scaling can replace the instance  

This allows systems to recover without manual intervention.

---

### 5. Security group chaining improves isolation

Rather than allowing traffic using IP ranges, security groups can reference other security groups.

In this setup:
- ALB security group allows inbound traffic from the internet  
- EC2 security group allows inbound traffic only from the ALB security group  

This ensures EC2 instances cannot be accessed directly.

---

### 6. Route 53 ALIAS records simplify DNS

Route 53 uses ALIAS records for AWS resources.

ALIAS records:
- point DNS names directly to load balancers  
- automatically update if IP addresses change  
- do not incur additional DNS query charges  

This removes the need to manage static IP addresses.

---

### 7. HTTPS is terminated at the load balancer

TLS encryption is handled by the ALB using AWS Certificate Manager.

Important points:
- ACM provides free public certificates  
- certificates automatically renew  
- HTTPS terminates at the ALB, not on EC2  

This simplifies certificate management significantly.

---

### 8. Auto Scaling Groups provide resilience

The Auto Scaling Group ensures:
- a desired number of instances is always running  
- failed instances are automatically replaced  
- new instances are registered with the target group automatically  

This demonstrated true self-healing infrastructure.

---

## Cost Awareness

This assignment highlighted how infrastructure costs can appear quickly.

Key cost considerations:
- Application Load Balancers are billed hourly  
- Route 53 hosted zones incur monthly charges  
- EBS volumes remain billable even after instances are stopped  

---

## Final Reflection

This assignment helped bridge the gap between theory and real-world cloud architecture.

I now clearly understand:
- why load balancers exist  
- how traffic is distributed safely  
- how health checks control availability  
- how Auto Scaling enables self-healing systems  
- how DNS and HTTPS integrate with AWS services  
