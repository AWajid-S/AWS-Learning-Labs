# Learning Notes – Assignment 1 (VPC & Networking)

## Overview

This assignment helped me understand how AWS networking components work together in a real environment rather than just theory.

I built a full VPC architecture from scratch and validated traffic flow between public and private resources.

---

## What I Built

- Custom VPC (10.0.0.0/16)
- Public subnet (10.0.1.0/24)
- Private subnet (10.0.2.0/24)
- Internet Gateway attached to VPC
- Elastic IP allocation
- NAT Gateway in public subnet
- Separate public and private route tables
- Public EC2 instance with public IPv4
- Private EC2 instance without public IP
- CloudWatch monitoring enabled on both instances

---

## Key Concepts I Learned

### 1. Subnets alone do nothing
A subnet is not public or private by default.

What actually defines this is:
- route table associations
- default routes (0.0.0.0/0)

---

### 2. Internet Gateway vs NAT Gateway

- Internet Gateway:
  - Used for inbound and outbound internet access
  - Required for public subnets

- NAT Gateway:
  - Allows outbound-only internet access
  - Prevents inbound traffic to private instances
  - One of the most expensive basic AWS services

---

### 3. Route tables control everything

Traffic flow is entirely decided by route tables.

Examples:
- Public subnet → 0.0.0.0/0 → IGW
- Private subnet → 0.0.0.0/0 → NAT Gateway

Without correct routing, resources appear “broken”.

---

### 4. Security groups are stateful

- Inbound rules control who can start communication
- Outbound responses are automatically allowed
- Using security-group-to-security-group rules is more secure than CIDR ranges

---

### 5. CloudWatch behavior

- Basic EC2 metrics are enabled automatically
- Metrics remain visible historically even after instance termination
- Billing stops when instances are terminated

---

## Cost Awareness

This lab showed me how quickly AWS networking costs can appear.

Key cost drivers:
- NAT Gateway hourly charge
- Elastic IP when not attached
- EC2 runtime

Important lesson:
Always delete NAT Gateways immediately after labs.

---

## Final Reflection

This assignment helped connect theory to real-world AWS architecture.

I now clearly understand:
- how traffic moves inside a VPC
- why private subnets exist
- how AWS enforces network isolation
- how costs can silently grow if resources are left running
