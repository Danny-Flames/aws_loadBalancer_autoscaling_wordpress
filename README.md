# WordPress on AWS: Capstone Project

## Project Overview
This project involves deploying a highly available, scalable, and secure WordPress site on AWS for a digital marketing agency, **DigitalBoost**. The solution leverages AWS services like VPC, EC2, RDS, EFS, Application Load Balancer (ALB), and Auto Scaling to meet the client's requirements.

---

## Architecture Overview
The solution architecture includes:
- **VPC**: A Virtual Private Cloud with public and private subnets for isolation.
- **EC2 Instances**: Hosting the WordPress application.
- **Amazon RDS**: A managed MySQL database for WordPress.
- **Amazon EFS**: A shared file system for storing WordPress files.
- **ALB and Auto Scaling**: To distribute traffic and scale resources automatically.

---

## Components and Implementation Steps

### **1. VPC Setup**
#### **1.1 Define IP Address Range**
- **CIDR Block**: `10.0.0.0/16`.
- **Subnets**:
  - Public Subnet: `10.0.1.0/24`.
  - Private Subnet: `10.0.2.0/24`.

#### **1.2 Create VPC**
- Created a custom VPC with the defined CIDR block.
- Enabled DNS hostname support.

#### **1.3 Configure Route Tables**
- Configured a public route table to route traffic to the internet gateway.
- Configured a private route table for NAT Gateway access.

---

### **2. Public and Private Subnets with NAT Gateway**
#### **2.1 Public Subnet**
- Assigned a public subnet for resources accessible from the internet.
- Associated it with the public route table.

#### **2.2 Private Subnet**
- Created a private subnet for backend resources like the RDS and EFS.
- Associated it with the private route table.

#### **2.3 NAT Gateway**
- Created a NAT Gateway in the public subnet to allow outbound internet access for resources in the private subnet.

---

### **3. AWS MySQL RDS Setup**
#### **3.1 Create RDS Instance**
- Engine: MySQL.
- Instance Type: `db.t2.micro`.
- Storage: 20GB General Purpose SSD.
- Multi-AZ deployment enabled for high availability.

#### **3.2 Configure Security Groups**
- Configured the security group to allow:
  - MySQL traffic (port `3306`) from the WordPress EC2 instances.

#### **3.3 WordPress-RDS Connection**
- Configured the WordPress `wp-config.php` file to use the RDS endpoint as the database host.

---

### **4. EFS Setup for WordPress Files**
#### **4.1 Create an EFS File System**
- Created an EFS file system with automatic backups enabled.
- Associated the file system with the VPC.

#### **4.2 Mount EFS on WordPress Instances**
- Mounted the EFS file system to the `/mnt/efs` directory.
- Migrated WordPress files to `/mnt/efs/wordpress`.
- Created a symbolic link from `/var/www/html` to `/mnt/efs/wordpress`.

#### **4.3 Configure WordPress**
- Updated file and directory permissions for seamless operation.
- Restarted the web server to ensure changes took effect.

---

### **5. Application Load Balancer and Auto Scaling**
#### **5.1 Create Application Load Balancer**
- Created an internet-facing ALB.
- Configured listeners for HTTP traffic on port `80`.
- Forwarded traffic to the target group.

#### **5.2 Create Target Group**
- Registered the WordPress EC2 instances to the target group.
- Configured health checks to monitor instance health.

#### **5.3 Create Auto Scaling Group**
- Configured an Auto Scaling Group with:
  - Minimum Instances: 1.
  - Desired Instances: 2.
  - Maximum Instances: 3.
- Attached the ALB to distribute traffic.
- Configured scaling policies based on average CPU utilization.

---

## Testing and Verification
1. Accessed the WordPress site using the ALB DNS name.
2. Verified:
   - WordPress functionality (uploads, plugins).
   - Database connection to RDS.
   - File system operations on EFS.
3. Simulated increased traffic using load testing tools:
   - Observed the Auto Scaling Group adding instances as needed.

---

## Security Measures
- **VPC Isolation**: Separated public and private resources.
- **NAT Gateway**: Secured private resources while allowing outbound access.
- **Security Groups**: Restricted access to instances and RDS.
- **EFS Permissions**: Ensured proper ownership and permissions for WordPress files.

---

## Cleanup Instructions
To avoid unnecessary costs, clean up resources after the project:
1. Terminate EC2 instances.
2. Delete the ALB and target group.
3. Delete the Auto Scaling Group.
4. Delete the EFS file system.
5. Delete the RDS instance.
6. Delete the VPC and associated resources.

---

## Future Improvements
- Enable HTTPS with an SSL certificate for the ALB.
- Use CloudFront for content delivery and caching.
- Implement AWS WAF for additional security.

---

## Author
**Ekpa Daniel Egwoke**

---

## Acknowledgements
Special thanks to the guides from the learning platform and also from AWS resources for supporting the project.
