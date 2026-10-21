# AWS Three-Tier Web Application Architecture

## 📌 Project Overview

This project demonstrates the design and implementation of a three-tier web application architecture on Amazon Web Services (AWS). The architecture separates the presentation, application, and database layers to improve security, scalability, and maintainability.

The application uses Nginx to serve the web interface, PHP and PHP-FPM to process application requests, and Amazon RDS for MySQL to store form submission data.

## 🏗️ Architecture

**Request Flow:**

`User Browser → Internet-Facing ALB → Nginx Web Tier → Internal ALB → PHP Application Tier → Amazon RDS MySQL`

The infrastructure is designed within a custom VPC across two Availability Zones in the Seoul Region (`ap-northeast-2`).

## 🛠️ AWS Services and Technologies

- **Amazon VPC:** Custom network using CIDR `10.0.0.0/16`
- **Amazon EC2:** Hosts the Nginx web servers and PHP application servers
- **Application Load Balancer:** Distributes traffic across web and application tiers
- **Auto Scaling Groups:** Manages the desired capacity of web and application instances
- **Amazon RDS for MySQL:** Managed relational database
- **Nginx:** Web server for serving HTML pages and forwarding PHP requests
- **PHP and PHP-FPM:** Application processing
- **Security Groups and Network ACLs:** Network traffic control
- **Amazon Machine Images (AMIs):** Supports repeatable server deployments

## 🌐 Network Design

The VPC uses eight `/20` subnets across two Availability Zones.

| Tier | Subnet CIDRs | Purpose |
|---|---|---|
| Public | `10.0.0.0/20`, `10.0.16.0/20` | Internet-facing resources |
| Web | `10.0.32.0/20`, `10.0.48.0/20` | Private Nginx servers |
| Application | `10.0.64.0/20`, `10.0.80.0/20` | Private PHP servers |
| Database | `10.0.96.0/20`, `10.0.112.0/20` | Private RDS subnets |

The remaining CIDR range, `10.0.128.0/17`, is reserved for future expansion.

## 🔄 Application Workflow

1. The user opens the web application in a browser.
2. The internet-facing Application Load Balancer forwards HTTP requests to a healthy Nginx web server.
3. Nginx serves HTML pages directly and forwards PHP requests to the internal Application Load Balancer.
4. The internal load balancer forwards requests to a healthy PHP application server.
5. PHP processes the submitted form and connects to Amazon RDS for MySQL.
6. The database stores the submitted information, and the response returns to the browser.

## 🔐 Security Design

- The internet-facing load balancer is the public entry point.
- Web servers reside in private subnets and accept HTTP traffic from the web load balancer.
- The internal load balancer accepts traffic from the web tier.
- PHP application servers accept traffic from the internal load balancer.
- RDS MySQL uses private database subnets and is configured for no public access.
- Database traffic uses port `3306` and is restricted to the application tier.

## 📈 Auto Scaling Configuration

Both the Web Tier and Application Tier are designed with the following Auto Scaling settings:

- **Desired capacity:** 2 instances
- **Minimum capacity:** 1 instance
- **Maximum capacity:** 4 instances

These settings provide a starting point for learning instance replacement, scaling, and multi-AZ distribution.

## 📚 Key Learnings

- Designing a three-tier cloud architecture
- Creating a custom VPC and subnet address plan
- Configuring public and private routing
- Working with internet-facing and internal load balancers
- Configuring Auto Scaling Groups and AMIs
- Applying tier-based security group rules
- Connecting PHP applications to Amazon RDS
- Troubleshooting load balancer health checks and HTTP errors

## 🚀 Future Improvements

- Enable and test Multi-AZ deployment for RDS
- Configure HTTPS using AWS Certificate Manager
- Store database credentials securely with AWS Secrets Manager
- Add CloudWatch monitoring and alarms
- Automate infrastructure deployment using Terraform or AWS CloudFormation

## ⚠️ Important

This repository documents a hands-on learning project. Features should be considered complete only after they have been configured and tested in AWS. Multi-AZ database failover is a planned improvement unless explicitly enabled.

**Built while learning AWS Cloud Architecture.**
