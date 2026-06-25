# AWS Lab 1 – Identity and Access Management (IAM)

## Overview

This lab introduced the fundamentals of **AWS Identity and Access Management (IAM)** by exploring how AWS controls authentication and authorization through users, groups, and policies. The objective was to understand the principle of least privilege by assigning different permission levels to users and observing how those permissions affected access to AWS resources.

---

## Objectives

* Understand the purpose of AWS IAM.
* Create and manage IAM users and groups.
* Assign managed policies to user groups.
* Apply role-based access control (RBAC).
* Test permission boundaries using different user accounts.

---

## AWS Services Used

* AWS Identity and Access Management (IAM)
* Amazon EC2
* Amazon S3

---

## Key Concepts

### Identity and Access Management (IAM)

IAM is AWS's authentication and authorization service that controls **who can access AWS resources** and **what actions they are allowed to perform**.

Key IAM components include:

* **Users** – Individual identities for people or applications.
* **Groups** – Collections of users with shared permissions.
* **Policies** – JSON documents defining allowed or denied actions.
* **Permissions** – Rules that determine access to AWS services.

This lab demonstrated how IAM implements the **Principle of Least Privilege**, ensuring users receive only the permissions required for their responsibilities.

---

## Lab Tasks

### 1. Reviewed Existing IAM Users

Three IAM users were provided:

* user-1
* user-2
* user-3

Initially, none of the users belonged to any groups or had permissions assigned.

---

### 2. Configured IAM Groups

Three security groups were available:

| Group       | Permission Level               |
| ----------- | ------------------------------ |
| EC2-Admin   | Full EC2 administrative access |
| EC2-Support | Read-only EC2 access           |
| S3-Support  | Read-only Amazon S3 access     |

Users were assigned according to their roles:

| User   | Assigned Group |
| ------ | -------------- |
| user-1 | S3-Support     |
| user-2 | EC2-Support    |
| user-3 | EC2-Admin      |

---

### 3. Examined IAM Policies

The attached AWS managed policies were inspected to understand how permissions are granted.

Examples included:

* AmazonS3ReadOnlyAccess
* AmazonEC2ReadOnlyAccess
* EC2 Administrator Policy

The policy JSON documents defined:

* Allowed actions
* Resources
* Effect (Allow/Deny)

---

### 4. Tested User Permissions

Each account was used to verify its assigned permissions.

Observations:

**user-1**

* Could access Amazon S3
* Could not manage EC2 resources

**user-2**

* Could view EC2 instances
* Unable to stop or modify instances

**user-3**

* Full EC2 administrative privileges
* Successfully stopped EC2 instances

This demonstrated how IAM policies are enforced immediately after assignment.

---

# Screenshots

## Screenshot 1

**IAM Users**

<img width="460" height="128" alt="image" src="https://github.com/user-attachments/assets/db9dbac0-a501-418e-9ce3-09f8bd4ec428" />


---

## IAM Groups

<img width="456" height="103" alt="image" src="https://github.com/user-attachments/assets/085e85bb-1893-4143-b26c-41018d43570b" />


---

## Screenshot 3

**IAM Policies**

> Insert screenshots of:

<img width="455" height="215" alt="image" src="https://github.com/user-attachments/assets/491b9bb5-080b-4eff-b55b-1c9bdb27314e" />

<img width="456" height="205" alt="image" src="https://github.com/user-attachments/assets/fe93c012-1a8f-4614-8849-96d6e27b831b" />

<img width="448" height="196" alt="image" src="https://github.com/user-attachments/assets/4b5a48de-4702-47ec-a4ad-4c3a6a3b3eb4" />


---

## Screenshot 4

**Permission Testing**

<img width="452" height="238" alt="image" src="https://github.com/user-attachments/assets/61f6f313-4899-4b52-8672-e184ac36001d" />


---

## Screenshot 5

**Administrator Access**

<img width="454" height="202" alt="image" src="https://github.com/user-attachments/assets/c18386dd-8ec8-40f4-9a59-c62af67a1266" />


---

# Skills Demonstrated

* AWS Identity and Access Management (IAM)
* Role-Based Access Control (RBAC)
* IAM Users and Groups
* AWS Managed Policies
* Principle of Least Privilege
* Permission Validation
* Cloud Identity Management

---

# What I Learned

This lab helped me understand how AWS secures cloud environments through centralized identity and permission management.

Rather than assigning permissions directly to users, AWS encourages assigning users to groups that inherit predefined policies. This simplifies administration while reducing configuration errors.

Testing multiple user accounts reinforced how IAM policies are enforced in real time and highlighted the importance of granting only the permissions necessary for a user's role.

I also gained a better understanding of the structure of IAM policy documents and how AWS evaluates permissions before allowing access to cloud resources.

---

# Challenges

The AWS Management Console contains many navigation panels, making it initially difficult to locate IAM users, groups, and policies.

Another challenge was understanding why certain EC2 actions were unavailable when logged in as different users. After reviewing the attached policies, it became clear that these restrictions were intentional and enforced through IAM permissions.

---

# Key Takeaways

* IAM is the foundation of AWS security.
* Groups simplify permission management.
* Policies define exactly what actions users can perform.
* AWS follows the Principle of Least Privilege to reduce security risk.
* Testing permissions from different user accounts is an effective way to validate access control configurations.
