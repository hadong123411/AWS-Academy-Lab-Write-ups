# AWS Lab 5 – Amazon Relational Database Service (RDS)

## Overview

In this lab, I deployed and configured an **Amazon Relational Database Service (RDS)** MySQL database within a Virtual Private Cloud (VPC). The lab focused on securing database connectivity using Security Groups and DB Subnet Groups, enabling high availability through Multi-AZ deployment, and connecting a web application hosted on Amazon EC2 to the managed database instance.

This exercise demonstrated how AWS simplifies database administration while maintaining secure communication between cloud services.

---

## Objectives

* Create an Amazon RDS MySQL database
* Configure database networking using a VPC
* Create and configure a DB Security Group
* Deploy a DB Subnet Group across multiple Availability Zones
* Understand Multi-AZ deployment
* Connect an EC2-hosted web application to an RDS database
* Test database functionality through a web application

---

# AWS Services Used

* Amazon RDS
* Amazon EC2
* Amazon VPC
* Security Groups
* DB Subnet Groups
* Multi-AZ Deployment
* MySQL

---

# Key Concepts

## Amazon Relational Database Service (RDS)

Amazon RDS is a managed database service that simplifies the deployment, maintenance, and scaling of relational databases.

Unlike self-managed databases running on EC2, Amazon RDS automates tasks such as:

* Software installation
* Database patching
* Backups
* Monitoring
* High availability
* Infrastructure maintenance

This allows developers to focus on applications rather than database administration.

---

## Security Groups

Security Groups act as virtual firewalls that control inbound and outbound traffic.

For databases, Security Groups restrict which resources are allowed to establish connections.

In this lab, inbound MySQL traffic (TCP Port 3306) was permitted only from the web server's Security Group, ensuring the database remained inaccessible from the public internet.

---

## DB Subnet Groups

A DB Subnet Group defines the private subnets where an Amazon RDS database may be deployed.

Using multiple subnets across different Availability Zones improves availability and supports Multi-AZ deployments.

---

## Multi-AZ Deployment

Multi-AZ deployments automatically maintain a standby database instance in another Availability Zone.

Benefits include:

* Improved availability
* Automatic failover
* Increased fault tolerance
* Reduced downtime during infrastructure failures

This architecture is commonly used for production environments.

---

## Database Endpoints

Unlike traditional database servers accessed through fixed IP addresses, Amazon RDS provides an endpoint.

Applications connect using:

* Endpoint
* Database name
* Username
* Password

AWS manages the underlying infrastructure while keeping the endpoint consistent.

---

# Lab Walkthrough

## Step 1 – Configure Database Security

A dedicated Security Group named **DB Security Group** was created within the Lab VPC.

The inbound rules were configured to allow:

| Protocol     | Port | Source             |
| ------------ | ---- | ------------------ |
| MySQL/Aurora | 3306 | Web Security Group |

Restricting access to only the web server improves database security by preventing unauthorized connections.

### Screenshot

<img width="445" height="414" alt="image" src="https://github.com/user-attachments/assets/a3525638-b71a-4af4-a911-9ed829fc078f" />


---

## Step 2 – Create a DB Subnet Group

A DB Subnet Group named:

```text
DB-Subnet-Group
```

was created using two private subnets located in separate Availability Zones.

This configuration enables Amazon RDS to support Multi-AZ deployment.

### Screenshot

<img width="449" height="651" alt="image" src="https://github.com/user-attachments/assets/8c5ec7f6-f038-42d1-ae0e-876b96020313" />


---

## Step 3 – Deploy the Amazon RDS Instance

An Amazon RDS MySQL instance was created with the following configuration:

| Setting             | Value                     |
| ------------------- | ------------------------- |
| Engine              | MySQL                     |
| Template            | Dev/Test                  |
| Deployment          | Multi-AZ                  |
| Instance Class      | db.t3.micro               |
| Storage             | 20 GB General Purpose SSD |
| Database Identifier | lab-db                    |
| Master Username     | main                      |

The Lab VPC and previously created DB Security Group were selected during deployment.

To reduce provisioning time for the lab environment, optional services such as automatic backups, enhanced monitoring, and encryption were disabled.

### Screenshot

<img width="417" height="356" alt="image" src="https://github.com/user-attachments/assets/19e551cb-d10b-414c-8a32-884ce8d0b714" />
<img width="440" height="276" alt="image" src="https://github.com/user-attachments/assets/cbbf4f5d-1a6b-447b-a2f5-8eb29ca097d1" />
<img width="328" height="344" alt="image" src="https://github.com/user-attachments/assets/2c9acfe5-2b1f-48f6-9741-10f3f278a3c2" />


---

## Step 4 – Connect the Web Application

The EC2 web application was opened in a web browser.

The following connection details were supplied:

| Setting  | Value        |
| -------- | ------------ |
| Endpoint | RDS Endpoint |
| Database | lab          |
| Username | main         |
| Password | lab-password |

After submitting the configuration, the web application successfully established a connection with the Amazon RDS database.

### Screenshot

<img width="352" height="170" alt="image" src="https://github.com/user-attachments/assets/c7735d57-c07a-4178-8e98-9e270ff5bee0" />


---

## Step 5 – Verify Database Functionality

The Address Book web application was used to validate database connectivity.

The following operations were successfully performed:

* Create new contacts
* Edit existing contacts
* Delete contacts

These actions confirmed that the application was communicating correctly with the managed MySQL database hosted on Amazon RDS.

### Screenshot

<img width="446" height="158" alt="image" src="https://github.com/user-attachments/assets/f7202e68-1ac8-466c-98b4-c1261595609f" />


---

# Skills Demonstrated

* Amazon RDS deployment
* MySQL database provisioning
* Amazon VPC networking
* Security Group configuration
* DB Subnet Group configuration
* Multi-AZ deployment
* Database endpoint configuration
* Cloud database administration
* Secure application connectivity

---

# What I Learned

This lab provided practical experience deploying and securing a managed relational database within AWS.

I learned how Amazon RDS abstracts much of the operational overhead associated with database management, including infrastructure provisioning and high availability.

Configuring Security Groups and DB Subnet Groups reinforced the importance of network isolation and controlled communication between cloud resources. Rather than exposing the database directly to the internet, only the EC2 web server was granted permission to connect over MySQL.

Connecting the web application to the database using the RDS endpoint demonstrated how managed databases integrate with cloud-hosted applications. Additionally, configuring a Multi-AZ deployment improved my understanding of high availability, fault tolerance, and production-ready cloud architectures.

---

# Challenges

The most challenging aspect of this lab was understanding how multiple AWS networking components interact to support secure database communication.

Initially, configuring Security Groups, DB Subnet Groups, and VPC networking required careful attention, as a single misconfiguration could prevent the web application from connecting to the database.

Another challenge was understanding the purpose of the RDS endpoint and how managed database services differ from traditional self-hosted database servers. Completing the lab improved my confidence in deploying secure, highly available cloud databases.

---

# Key Takeaways

* Amazon RDS simplifies relational database management through automation.
* Security Groups protect databases by restricting network access.
* DB Subnet Groups enable secure deployment within private subnets.
* Multi-AZ deployment improves availability and fault tolerance.
* Applications connect to RDS using managed database endpoints rather than fixed server IP addresses.
* Secure networking is essential for protecting cloud-hosted databases while enabling communication between AWS services.
