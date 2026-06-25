# AWS Lab 6 – Auto Scaling & Load Balancing

## Overview

In this lab, I implemented a highly available and scalable web application architecture using **Amazon EC2 Auto Scaling**, **Application Load Balancer (ALB)**, and **Amazon CloudWatch**. The environment automatically adjusted the number of EC2 instances based on CPU utilization while distributing incoming traffic across multiple instances.

This lab demonstrated how AWS can automatically respond to changing workloads, improving application availability, fault tolerance, and operational efficiency without manual intervention.

---

## Objectives

* Create an Amazon Machine Image (AMI)
* Configure an Application Load Balancer
* Create a Target Group
* Create a Launch Template
* Deploy an Auto Scaling Group
* Configure dynamic scaling policies
* Monitor scaling activity using CloudWatch
* Verify high availability and automatic instance replacement

---

# AWS Services Used

* Amazon EC2
* Amazon Machine Image (AMI)
* Application Load Balancer (ALB)
* Target Groups
* Auto Scaling Groups
* Launch Templates
* Amazon CloudWatch

---

# Key Concepts

## Amazon Machine Image (AMI)

An Amazon Machine Image (AMI) is a reusable template containing the operating system, applications, and configuration required to launch EC2 instances.

Using an AMI ensures that every instance launched by Auto Scaling is configured identically.

---

## Application Load Balancer (ALB)

An Application Load Balancer distributes incoming application traffic across multiple EC2 instances.

Benefits include:

* Improved availability
* Even traffic distribution
* Fault tolerance
* Increased scalability

If one instance becomes unavailable, traffic is automatically redirected to healthy instances.

---

## Target Groups

A Target Group contains the EC2 instances that receive traffic from the Load Balancer.

Health checks continuously monitor each instance to ensure requests are only sent to healthy targets.

---

## Launch Templates

A Launch Template defines the configuration Auto Scaling uses when creating new EC2 instances.

It includes settings such as:

* Amazon Machine Image (AMI)
* Instance type
* Security Groups
* Storage configuration
* Monitoring settings

This ensures newly launched instances remain consistent across the environment.

---

## Auto Scaling Groups

Auto Scaling Groups automatically launch or terminate EC2 instances based on application demand.

Benefits include:

* High availability
* Fault tolerance
* Cost optimization
* Elastic scalability

---

## Amazon CloudWatch

CloudWatch continuously monitors AWS resources and collects performance metrics.

Scaling policies use CloudWatch metrics, such as CPU utilization, to automatically adjust infrastructure according to workload demand.

---

# Lab Walkthrough

## Step 1 – Create an Amazon Machine Image (AMI)

An AMI was created from the existing **Web Server 1** EC2 instance.

This image served as the template used by Auto Scaling to provision identical web servers whenever additional capacity was required.

### Screenshot

<img width="455" height="544" alt="image" src="https://github.com/user-attachments/assets/847554ae-2f54-48fd-a2f5-17f1682c894d" />


---

## Step 2 – Create a Target Group

A Target Group named:

```text
LabGroup
```

was created within the Lab VPC.

This Target Group would later receive incoming traffic from the Application Load Balancer.

### Screenshot

<img width="458" height="277" alt="image" src="https://github.com/user-attachments/assets/b6740d7f-cf9c-4630-8559-400c5db7fe89" />


---

## Step 3 – Deploy an Application Load Balancer

An **Application Load Balancer (ALB)** named:

```text
LabELB
```

was deployed.

Configuration included:

* Public subnets
* Web Security Group
* HTTP Listener
* Forwarding requests to the Target Group

The Load Balancer was configured to distribute incoming requests evenly across healthy EC2 instances.

### Screenshot

<img width="457" height="271" alt="image" src="https://github.com/user-attachments/assets/f1a5e68f-9ec5-451c-ac6f-7f74ed1f2716" />


---

## Step 4 – Create a Launch Template

A Launch Template named:

```text
LabConfig
```

was created using the previously generated AMI.

Configuration included:

* AMI
* t2.micro instance type
* Detailed CloudWatch monitoring
* Existing Security Group

The Launch Template ensures every new EC2 instance is deployed using the same configuration.

### Screenshot

<img width="457" height="509" alt="image" src="https://github.com/user-attachments/assets/6d216dc3-1c2e-4981-bec8-1c8923818407" />


---

## Step 5 – Create an Auto Scaling Group

An Auto Scaling Group named:

```text
Lab Auto Scaling Group
```

was created using the Launch Template.

Configuration included:

| Setting          | Value       |
| ---------------- | ----------- |
| Minimum Capacity | 2 Instances |
| Maximum Capacity | 6 Instances |

The Auto Scaling Group was attached to the previously created Target Group, allowing new instances to automatically begin receiving traffic.

### Screenshot

<img width="453" height="455" alt="image" src="https://github.com/user-attachments/assets/7eabab8e-15cd-431f-bde3-852b60cadce9" />


---

## Step 6 – Verify Load Balancing

Once deployment completed, two EC2 instances were automatically launched.

Health checks confirmed both instances were healthy, allowing the Application Load Balancer to begin distributing incoming traffic between them.

This demonstrated that the environment could continue serving users even if one instance became unavailable.

### Screenshot

<img width="458" height="524" alt="image" src="https://github.com/user-attachments/assets/7549b738-d059-4011-b30b-fc1a49cb2b61" />


---

## Step 7 – Configure Dynamic Scaling

The target CPU utilization threshold was adjusted from **60%** to **50%**.

CloudWatch alarms continuously monitored CPU usage and automatically triggered scaling events whenever the threshold was exceeded.

This enabled the infrastructure to dynamically increase capacity during periods of higher workload.

### Screenshot

<img width="458" height="443" alt="image" src="https://github.com/user-attachments/assets/3e6c131c-de36-4649-beeb-476588411fc8" />


---

## Step 8 – Test Automatic Scaling

A workload was generated to increase CPU utilization.

As CPU usage exceeded the configured threshold, the Auto Scaling Group automatically launched additional EC2 instances.

This demonstrated AWS's ability to scale infrastructure without requiring manual intervention.


---

## Step 9 – Verify High Availability

The original **Web Server 1** instance was terminated.

Despite the termination, the Auto Scaling Group automatically provisioned a replacement instance using the Launch Template and AMI.

The Load Balancer continued routing traffic to healthy instances, maintaining application availability without service interruption.

### Screenshot

<img width="454" height="584" alt="image" src="https://github.com/user-attachments/assets/c6d3167a-3f89-4225-ba7d-ef35ce04f412" />


---

# Skills Demonstrated

* Amazon EC2
* Amazon Machine Images (AMI)
* Application Load Balancer
* Target Groups
* Launch Templates
* Auto Scaling Groups
* Amazon CloudWatch
* Dynamic infrastructure scaling
* High availability architecture
* Fault tolerance
* Cloud infrastructure automation

---

# What I Learned

This lab demonstrated how multiple AWS services integrate to create a resilient and scalable cloud architecture.

Rather than manually provisioning additional servers during periods of high demand, Auto Scaling Groups automatically responded to CloudWatch metrics by launching or terminating EC2 instances as required. This highlighted how cloud platforms can dynamically adapt to changing workloads while optimizing resource usage.

I also gained practical experience configuring Application Load Balancers, Target Groups, and Launch Templates. Understanding how these services interact reinforced the importance of automation, redundancy, and high availability when designing production-ready cloud environments.

Perhaps the most valuable takeaway was observing the Auto Scaling Group automatically replace a terminated EC2 instance. This demonstrated how AWS maintains service availability through self-healing infrastructure, reducing downtime and improving application resilience.

---

# Challenges

The most challenging aspect of this lab was understanding how the Application Load Balancer, Target Groups, Launch Templates, Auto Scaling Groups, and CloudWatch alarms interact as a complete system.

Because each service depends on the correct configuration of the others, troubleshooting required careful attention to resource relationships and deployment settings. Navigating the AWS Management Console also required familiarity with multiple services and configuration pages.

Completing the lab strengthened my understanding of cloud automation and improved my confidence in designing scalable, fault-tolerant architectures.

---

# Key Takeaways

* Amazon Machine Images enable consistent EC2 deployments.
* Application Load Balancers distribute traffic across healthy instances.
* Target Groups manage traffic routing and health checks.
* Launch Templates standardize EC2 instance configuration.
* Auto Scaling Groups automatically adjust infrastructure based on workload demand.
* Amazon CloudWatch provides the monitoring required to trigger automated scaling events.
* Combining these services creates highly available, fault-tolerant, and production-ready cloud architectures.
