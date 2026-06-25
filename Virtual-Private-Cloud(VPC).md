# AWS Lab 2 – Building a Secure Virtual Private Cloud (VPC)

## Overview

In this lab, I designed and deployed a custom **Amazon Virtual Private Cloud (VPC)** from scratch. The environment included public and private subnets, route tables, an Internet Gateway, a NAT Gateway, security groups, and an EC2 web server.

The objective was to understand how AWS networking components work together to securely control traffic flow between internet-facing resources and private infrastructure.

---

## Objectives

* Design a custom Virtual Private Cloud (VPC)
* Configure public and private subnets
* Create and configure route tables
* Deploy an Internet Gateway and NAT Gateway
* Configure Security Groups
* Launch an EC2 instance inside the VPC
* Validate network connectivity by hosting a web server

---

# AWS Services Used

* Amazon VPC
* Amazon EC2
* Internet Gateway
* NAT Gateway
* Route Tables
* Security Groups
* Elastic IP
* Public DNS

---

# Key Concepts

## Amazon VPC

Amazon Virtual Private Cloud (VPC) allows organizations to create an isolated virtual network inside AWS where cloud resources can be securely deployed.

A VPC provides complete control over:

* IP address ranges
* Subnet design
* Internet connectivity
* Routing
* Network security

---

## Public vs Private Subnets

### Public Subnet

Resources inside a public subnet can communicate directly with the internet through an **Internet Gateway**.

Typical resources include:

* Web servers
* Load Balancers
* Bastion Hosts

---

### Private Subnet

Resources inside a private subnet are isolated from direct internet access.

They commonly host:

* Databases
* Internal applications
* Backend services

Private resources can still download updates using a **NAT Gateway** without exposing themselves to inbound internet traffic.

---

## Internet Gateway

An Internet Gateway (IGW) enables communication between a VPC and the public internet.

Without an Internet Gateway, instances inside public subnets cannot receive or send internet traffic.

---

## NAT Gateway

A NAT Gateway allows resources inside private subnets to access the internet for outbound traffic while preventing inbound internet connections.

This is commonly used for:

* Installing software updates
* Downloading packages
* Accessing AWS services securely

---

## Route Tables

Route tables determine where network traffic should be sent.

For example:

| Destination | Target           |
| ----------- | ---------------- |
| 0.0.0.0/0   | Internet Gateway |
| 0.0.0.0/0   | NAT Gateway      |

Correct route table configuration is essential for proper network communication.

---

## Security Groups

Security Groups function as stateful virtual firewalls that control inbound and outbound traffic for AWS resources.

In this lab, a Security Group was configured to allow inbound HTTP traffic so the deployed web server could be accessed through its public DNS.

---

# Lab Walkthrough

## Step 1 – Create a Virtual Private Cloud

A custom VPC was created using the CIDR block:

```
10.0.0.0/16
```

This provided enough IP address space to divide the environment into multiple subnets.

### Screenshot

<img width="448" height="218" alt="image" src="https://github.com/user-attachments/assets/35cb7e63-45ba-4f57-8100-113823f66942" />


---

## Step 2 – Configure Public and Private Subnets

Two primary subnets were created:

| Subnet  | CIDR        |
| ------- | ----------- |
| Public  | 10.0.0.0/24 |
| Private | 10.0.1.0/24 |

Additional subnets were later created across another Availability Zone to improve redundancy.

### Screenshot

<img width="412" height="332" alt="image" src="https://github.com/user-attachments/assets/a2b7fab8-7a9d-456e-be3d-8ffc3117fcd1" />


---

## Step 3 – Configure Internet and NAT Gateways

An **Internet Gateway** was attached to the VPC, enabling internet access for public resources.

A **NAT Gateway** was then deployed so that instances inside private subnets could initiate outbound internet connections without becoming publicly accessible.

### Screenshot

<img width="422" height="370" alt="image" src="https://github.com/user-attachments/assets/492d7a96-3a5e-4690-92ff-004f3e21083c" />


---

## Step 4 – Configure Route Tables

Separate route tables were configured for public and private networking.

### Public Route Table

* Default route (0.0.0.0/0)
* Target: Internet Gateway

### Private Route Table

* Default route (0.0.0.0/0)
* Target: NAT Gateway

Each subnet was associated with its corresponding route table.

### Screenshot

<img width="458" height="427" alt="image" src="https://github.com/user-attachments/assets/ff14d34f-ad2f-42de-8db4-0b55bc1fad81" />

<img width="457" height="366" alt="image" src="https://github.com/user-attachments/assets/ac0c20cf-4c47-4eda-9ece-449034395098" />



---

## Step 5 – Create a Security Group

A Security Group was created to permit inbound HTTP (TCP Port 80) traffic.

This allows external users to access the hosted web application while maintaining a controlled security posture.

### Screenshot

<img width="468" height="545" alt="image" src="https://github.com/user-attachments/assets/8a0c42bd-79e1-4a75-a1ee-fe901dcdccbc" />


---

## Step 6 – Launch an EC2 Instance

An Amazon EC2 instance was deployed inside the public subnet.

During deployment:

* Public IP assignment was enabled
* User Data was used to automatically install a web server
* Security Group was attached

The automated User Data script provisioned the web server during instance startup.

### Screenshot

<img width="458" height="193" alt="image" src="https://github.com/user-attachments/assets/0366421d-fa49-4edc-a06c-82cdfa1dc655" />


---

## Step 7 – Verify Network Connectivity

After deployment completed, the EC2 instance was accessed using its **Public DNS**.

The hosted web page loaded successfully, confirming:

* Internet Gateway configuration
* Route table configuration
* Security Group rules
* EC2 deployment
* Web server installation

### Screenshot

<img width="431" height="227" alt="image" src="https://github.com/user-attachments/assets/49a2b15c-5a6f-40e9-a376-5d9b4d91eae5" />


---

# Skills Demonstrated

* Amazon VPC configuration
* CIDR planning
* Public and private subnet design
* Internet Gateway deployment
* NAT Gateway configuration
* Route table management
* Security Group configuration
* EC2 deployment
* Cloud network troubleshooting
* Network segmentation

---

# What I Learned

This lab significantly improved my understanding of AWS networking fundamentals.

Rather than simply launching an EC2 instance, I learned how multiple networking components work together to create a secure cloud environment. Designing separate public and private subnets reinforced the importance of network segmentation, while configuring route tables demonstrated how traffic is directed throughout a VPC.

I also gained practical experience configuring Internet and NAT Gateways, allowing me to understand how private resources can securely access the internet without exposing themselves to inbound connections.

Deploying an EC2 instance and successfully accessing the hosted web application validated the complete network configuration and demonstrated how infrastructure components integrate within AWS.

---

# Challenges

The most challenging aspect of this lab was understanding the relationship between route tables, subnet associations, and gateway configurations.

Because multiple networking resources depend on one another, a single configuration mistake can prevent connectivity. Navigating the AWS Management Console also required careful attention, particularly when associating subnets with the correct route tables.

Troubleshooting these issues improved my confidence in designing cloud network architectures.

---

# Key Takeaways

* Amazon VPC provides isolated cloud networking environments.
* Public and private subnets improve security through network segmentation.
* Internet Gateways enable inbound and outbound internet connectivity.
* NAT Gateways provide secure outbound internet access for private resources.
* Route tables control traffic flow throughout a VPC.
* Security Groups act as stateful virtual firewalls protecting AWS resources.
* Building a VPC from scratch provides a strong foundation for designing secure cloud infrastructure.
