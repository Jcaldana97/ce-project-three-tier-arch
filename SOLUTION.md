# Project Requirements

## Network Infrastructure (25%)

- [x] VPC with /16 CIDR block

- [x] 6 subnets across 2 Availability Zones:
	- [x] 2 public subnets (presentation tier)
	- [x] 2 private subnets (application tier)
	- [x] 2 private subnets (data tier)

- [x] Internet Gateway attached to VPC
- [x] NAT Gateway (at least 1) for private subnet internet access
- [x] Route tables properly configured

## Tier 1: Presentation Layer (15%)

- [x] Application Load Balancer (internet-facing)
- [x] Deployed in public subnets across 2 AZs
- [x] Listener on port 80 (HTTP)
- [x] Health check configured

## Tier 2: Application Layer (20%)

- [x] Minimum 3 EC2 instances running web application
- [x] Deployed in private subnets across 2 AZs
- [x] Registered with ALB target group

Application displays:
	- [x] Instance ID
	- [x] Availability Zone
	- [x] Database connection status
	- [x] Health check endpoint (/health)

## Tier 3: Data Layer (10%)

- [ ] Database placeholder (can use EC2 with simulated DB) OR RDS database (bonus points)
- [ ] Deployed in isolated private subnet
- [ ] Only accessible from application tier

## Security Configuration (20%)

Security groups for each tier:
- [x] ALB SG: Allow 80/443 from 0.0.0.0/0
- [x] App SG: Allow 80/443 from ALB SG only
- [x] Data SG: Allow DB port from App SG only
- [x] Principle of least privilege applied
- [x] No direct internet access to private tiers

## Documentation (10%)

- [ ] Architecture diagram (clear and professional)

README with:
- [ ] Architecture overview
- [ ] Design decisions and trade-offs
- [ ] Security strategy
- [ ] Testing results
- [ ] Cost breakdown