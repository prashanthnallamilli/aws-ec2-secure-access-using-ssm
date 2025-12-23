# aws-ec2-secure-access-using-ssm
Secure EC2 access using AWS Systems Manager (SSM) by eliminating SSH and implementing IAM-based Zero Trust access.

## Project Overview
In traditional cloud setups, EC2 instances are accessed using SSH keys and open inbound ports, which increases the attack surface and operational risk.
This project demonstrates how to securely manage an Amazon EC2 instance using **AWS Systems Manager (SSM)** by:
- Eliminating SSH access entirely
- Removing the need for key pairs (.pem files)
- Using IAM-based identity and access management
- Following Zero Trust security principles
This approach reflects how modern enterprises securely operate production servers in AWS.

## Why This Project Matters
Many beginners access EC2 instances using SSH and public IPs, which is insecure and not aligned with real-world cloud practices.
This project demonstrates how modern organizations:
- Eliminate SSH entirely
- Use identity-based access instead of network trust
- Reduce attack surface by default
- Apply Zero Trust principles in AWS

## Before vs After
### Before (Traditional Approach)
- SSH (port 22) open
- Public IP required
- Key pair (.pem) based access
- Higher attack surface
### After (This Project)
- No SSH access
- No inbound admin ports
- IAM + SSM based access
- Zero Trust security model

## Architecture Overview
The architecture follows a no-cost (AWS Free Tier) design:
- Amazon EC2 (Ubuntu instance)
- IAM Role attached to EC2
- AWS Systems Manager (Session Manager)
- No Inbound SSH Access
- No public key authentication

 User (IAM Identity)
        |
        v
AWS Systems Manager (SSM)
        |
        v
EC2 Instance (No SSH, No Open Ports)

## Security Design Decisions

### 1. SSH Completely Disabled
- Port 22 is removed from the security group
- EC2 cannot be accessed via SSH from the internet
- Eliminates brute-force and key compromise risks

### 2. IAM-Based Access (Zero Trust)
- Access to EC2 is controlled by IAM permissions
- No network-based trust
- Every session is authenticated and authorized

### 3. No Public Key Files
- No `.pem` files required
- Reduces key leakage and rotation overhead

### 4. Full Auditability
- All access via SSM is logged in AWS
- Supports compliance and traceability

##  Production-Style Architecture Design
In real-world cloud environments, application servers are not exposed directly to the internet.

A typical production architecture follows this design:

User (Browser)
     |
     v
Application Load Balancer (Public Subnet)
     |
     v
EC2 Application Servers (Private Subnet, No Public IP)
     |
     v
Administrative Access via AWS Systems Manager (SSM)

Key characteristics:
- EC2 instances do not have public IP addresses
- Only the Load Balancer is internet-facing
- Administrative access is identity-based (IAM + SSM)
- Network-level isolation using private subnets

## Why This Architecture Is Used in Companies
This architecture is widely adopted because it:
- Eliminates direct internet exposure of servers
- Reduces attack surface by design
- Centralizes security controls
- Supports Zero Trust access models
- Allows easy scaling without redesign

Even small applications benefit from this architecture due to improved security and future readiness.

## Implementation Steps

1. Launched an Ubuntu EC2 instance (AWS Free Tier)
2. Verified and enabled the Amazon SSM Agent
3. Created an IAM role with `AmazonSSMManagedInstanceCore` policy
4. Attached the IAM role to the EC2 instance
5. Connected to the instance using Session Manager
6. Removed SSH (port 22) from the security group
7. Verified that SSH access was no longer possible

## Security Group Configuration
Inbound rules after hardening:
- No SSH (port 22 removed)
- No admin ports exposed
- Only required application ports (if any)
This ensures the instance is not directly reachable from the internet.

## Verification
- SSH connection fails after removing port 22
- EC2 remains fully accessible via AWS Session Manager
- `whoami` returns `ssm-user`, confirming SSM-based access
- Instance visible as "Managed Node" in AWS Systems Manager

## Screenshots 
- EC2 instance visible as Managed Node in Systems Manager
- Successful Session Manager connection
- Security Group with SSH removed


## What This Project Demonstrates
- Practical understanding of AWS Systems Manager
- Secure EC2 access without SSH
- Zero Trust security concepts in cloud
- IAM-based access control
- Real-world cloud security best practices

## Cost Considerations
This project was implemented entirely within the AWS Free Tier.
No paid services such as Load Balancers, NAT Gateways, or ECS were used.

## Limitations & Next Steps
Limitations:
- Instance is accessed directly via SSM (no load balancer)
- Single EC2 instance (no high availability)
Planned Improvements:
- Place EC2 in a private subnet
- Add Application Load Balancer
- Enable HTTPS (TLS)
- Containerize application using Docker
- Deploy using ECS








