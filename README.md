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

## Architecture Overview
The architecture is no cost (AWS Free Tier) Design:
- Amazon EC2 (Ubuntu instance)
- IAM Role attached to EC2
- AWS Systems Manager(Session Manager)
- No Inbound SSH Access
- No public key authenticaion

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

## Implementation Steps

1. Launched an Ubuntu EC2 instance (AWS Free Tier)
2. Verified and enabled the Amazon SSM Agent
3. Created an IAM role with `AmazonSSMManagedInstanceCore` policy
4. Attached the IAM role to the EC2 instance
5. Connected to the instance using Session Manager
6. Removed SSH (port 22) from the security group
7. Verified that SSH access was no longer possible

## Verification

- SSH connection fails after removing port 22
- EC2 remains fully accessible via AWS Session Manager
- `whoami` returns `ssm-user`, confirming SSM-based access
- Instance visible as "Managed Node" in AWS Systems Manager

## What This Project Demonstrates

- Practical understanding of AWS Systems Manager
- Secure EC2 access without SSH
- Zero Trust security concepts in cloud
- IAM-based access control
- Real-world cloud security best practices

## Cost Considerations

This project was implemented entirely within the AWS Free Tier.
No paid services such as Load Balancers, NAT Gateways, or ECS were used.







