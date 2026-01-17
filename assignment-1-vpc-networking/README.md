# Assignment 1 — VPC & Networking

## Objective
Design and deploy a custom AWS VPC with public and private subnets, configure secure internet access using an Internet Gateway and NAT Gateway, and deploy EC2 instances.



## Architecture Overview
This lab implements a basic two-tier network architecture:

- Custom VPC (10.0.0.0/16)
- Public subnet for internet-facing resources
- Private subnet for internal resources
- Internet Gateway for inbound/outbound public traffic
- NAT Gateway for private subnet outbound access
- Separate route tables for traffic control

  
## Network Design
- VPC CIDR: 10.0.0.0/16
- Public Subnet: 10.0.1.0/24
- Private Subnet: 10.0.2.0/24

The public subnet allows internet access, while the private subnet is isolated and only allows outbound traffic via NAT Gateway.

## Routing & Internet Access
Public Route Table:
- 0.0.0.0/0 → Internet Gateway

Private Route Table:
- 0.0.0.0/0 → NAT Gateway

This ensures:
- Public EC2 instances are reachable from the internet
- Private EC2 instances cannot receive inbound internet traffic
  
## EC2 Deployment
Public EC2:
- Launched in public subnet
- Auto-assigned public IPv4 address
- Used for SSH access and testing

Private EC2:
- Launched in private subnet
- No public IP assigned
- Internet access provided only through NAT Gateway
  
## Security Design
Public EC2 Security Group:
- SSH (22) allowed only from my IP
- HTTP (80) allowed from my IP

Private EC2 Security Group:
- SSH allowed only from public EC2 security group
- No inbound internet access

This follows the principle of least privilege.

## Testing & Validation
- Verified public EC2 connectivity via SSH
- Confirmed private EC2 had no public IP
- Verified private EC2 outbound internet access through NAT Gateway
- Confirmed route tables associated with correct subnets
  
## Cost Considerations
Resources were created temporarily for learning purposes and deleted after testing.

Main cost contributors:
- NAT Gateway (hourly charge)
- EC2 instances (t2.micro)

Total cost was minimal due to short usage duration.

## Screenshots
Screenshots documenting each step are available in the `/screenshots` directory, including:

- VPC creation
- Subnet configuration
- Internet Gateway attachment
- NAT Gateway setup
- Route tables
- EC2 instances
- CloudWatch monitoring

## What I Learned
- How AWS networking components work together
- Difference between public and private subnets
- How NAT Gateway enables outbound-only access
- Importance of route tables in traffic flow
- How AWS pricing is heavily influenced by networking services
- How to safely clean up resources to avoid ongoing costs
