# AWS Lab 3 – Amazon Elastic Block Store (EBS) & Snapshots

## Overview

In this lab, I explored how **Amazon Elastic Block Store (EBS)** provides persistent block storage for Amazon EC2 instances. The lab focused on creating an EBS volume, attaching it to an EC2 instance, formatting and mounting the storage using Linux, configuring persistent storage, and creating snapshots for backup and disaster recovery.

This exercise demonstrated how cloud storage extends beyond simply provisioning resources in AWS—it also requires operating system configuration before the storage becomes usable.

---

## Objectives

* Create and attach an Amazon EBS volume
* Configure storage using Linux commands
* Format and mount a new file system
* Configure persistent storage using `/etc/fstab`
* Create EBS snapshots
* Restore data from a snapshot
* Verify successful data recovery

---

# AWS Services Used

* Amazon EC2
* Amazon Elastic Block Store (EBS)
* Amazon EBS Snapshots
* AWS Systems Manager Session Manager
* Amazon Linux

---

# Key Concepts

## Amazon Elastic Block Store (EBS)

Amazon EBS provides persistent block-level storage for Amazon EC2 instances.

Unlike instance store storage, EBS volumes persist independently of the EC2 instance and can be detached, reattached, backed up, and restored.

Common use cases include:

* Operating system disks
* Database storage
* Application data
* Persistent file systems

---

## Block Storage

Block storage presents storage as raw disk devices to the operating system.

Before it can be used, the operating system must:

1. Detect the device
2. Format it with a file system
3. Mount it
4. Configure persistence

This differs from object storage services such as Amazon S3.

---

## Mounting a File System

Mounting associates a storage device with a directory in Linux.

In this lab, the EBS volume was mounted to:

```bash
/mnt/data-store
```

Once mounted, applications could read and write files as if the storage were part of the local filesystem.

---

## /etc/fstab

The `/etc/fstab` file stores permanent mount configurations.

Without updating this file, mounted storage disappears after a reboot.

Adding the EBS volume to `/etc/fstab` ensured the storage automatically mounted whenever the EC2 instance restarted.

---

## Amazon EBS Snapshots

Snapshots create incremental backups of EBS volumes.

They can be used to:

* Recover deleted data
* Restore corrupted volumes
* Clone storage
* Support disaster recovery

Snapshots are stored in Amazon S3 behind the scenes but are managed entirely through the EBS service.

---

# Lab Walkthrough

## Step 1 – Create an EBS Volume

A new Amazon EBS volume was created with the following configuration:

| Property          | Value          |
| ----------------- | -------------- |
| Size              | 1 GiB          |
| Type              | gp2            |
| Availability Zone | Same AZ as EC2 |

The volume was tagged for easier identification.

### Screenshot

<img width="471" height="284" alt="image" src="https://github.com/user-attachments/assets/154a9ac9-363f-4834-873a-02a146ce27fa" />

<img width="458" height="458" alt="image" src="https://github.com/user-attachments/assets/61b912d8-a5fc-496f-b1e3-3239e93e9215" />

---

## Step 2 – Attach the Volume

The EBS volume was attached to the existing EC2 instance using the device name:

```bash
/dev/sdb
```

Attaching the volume makes it visible to the operating system, but it is not yet ready for use.

### Screenshot

<img width="471" height="582" alt="image" src="https://github.com/user-attachments/assets/8a55f680-825f-415a-8833-c40a5d48cc6f" />


---

## Step 3 – Connect Using Session Manager

AWS Systems Manager Session Manager was used to securely connect to the EC2 instance without requiring SSH access.

The existing storage configuration was verified using:

```bash
df -h
```

This confirmed that the newly attached EBS volume had not yet been mounted.

### Screenshot

<img width="471" height="233" alt="image" src="https://github.com/user-attachments/assets/084982a5-3c3d-4371-ab1f-373e65697ba6" />


---

## Step 4 – Format the Volume

The new storage device was formatted using the ext3 filesystem.

```bash
sudo mkfs -t ext3 /dev/sdb
```

Formatting prepares the raw storage so that Linux can organize files and directories.

### Screenshot

<img width="470" height="206" alt="image" src="https://github.com/user-attachments/assets/4d46ef19-ba3f-453b-a5d7-0e66844a7393" />


---

## Step 5 – Mount the Volume

A mount point was created:

```bash
sudo mkdir /mnt/data-store
```

The EBS volume was then mounted:

```bash
sudo mount /dev/sdb /mnt/data-store
```

The updated storage configuration was verified using:

```bash
df -h
```

### Screenshot

<img width="472" height="164" alt="image" src="https://github.com/user-attachments/assets/21a4db4a-fdc5-4d17-845a-603eaf52bc77" />


---

## Step 6 – Configure Persistent Storage

To ensure the storage remained available after reboot, the volume was added to:

```bash
/etc/fstab
```

The configuration was verified by viewing the file.

Without this step, Linux would not automatically mount the EBS volume during startup.

### Screenshot

<img width="227" height="20" alt="image" src="https://github.com/user-attachments/assets/1cb61169-3533-4379-8e12-579a553aefd1" />


---

## Step 7 – Verify Storage Functionality

A test file was created within the mounted directory.

```bash
echo "some text has been written"
```

The file was successfully read back, confirming that the EBS volume was functioning correctly.

### Screenshot

<img width="473" height="44" alt="image" src="https://github.com/user-attachments/assets/b680601b-cebf-4fa7-b9ce-33a179ebf009" />


---

## Step 8 – Create an EBS Snapshot

A snapshot of the configured EBS volume was created.

Snapshots provide incremental backups that can later be restored into entirely new EBS volumes.

### Screenshot

<img width="478" height="192" alt="image" src="https://github.com/user-attachments/assets/3558a001-1e6f-4e5e-83ec-287fed6d48a9" />

---

## Step 9 – Restore from Snapshot

A new EBS volume was created using the previously created snapshot.

The restored volume was attached to the EC2 instance using:

```bash
/dev/sdc
```

A second mount directory was created:

```bash
/mnt/data-store2
```

After mounting the restored volume, the original data was successfully recovered.

This demonstrated that EBS snapshots provide an effective mechanism for backup and disaster recovery.

### Screenshot

<img width="473" height="506" alt="image" src="https://github.com/user-attachments/assets/c49f04dc-c94f-4fa5-9fa1-1598805d83f8" />
<img width="473" height="387" alt="image" src="https://github.com/user-attachments/assets/340bd2ec-a4e7-4311-a06d-81bfb128ec36" />
<img width="478" height="194" alt="image" src="https://github.com/user-attachments/assets/ecb2a1cb-9d07-4f4d-8eb9-0710cf07d779" />


---

# Skills Demonstrated

* Amazon Elastic Block Store (EBS)
* Linux storage administration
* File system formatting
* Mounting storage volumes
* Persistent storage configuration
* Linux `/etc/fstab`
* AWS Systems Manager
* Snapshot creation
* Backup and disaster recovery
* Storage restoration

---

# What I Learned

This lab provided valuable hands-on experience managing persistent storage within AWS.

I learned that provisioning an EBS volume through the AWS Management Console is only the first step. The operating system must also recognize, format, and mount the storage before it becomes usable.

Configuring `/etc/fstab` reinforced the importance of persistence across system reboots, while creating and restoring snapshots demonstrated how AWS supports backup and disaster recovery without requiring manual file copying.

Perhaps the most important takeaway was understanding how cloud infrastructure and operating systems work together. Although AWS provisions the storage resource, Linux is responsible for preparing and managing the filesystem.

---

# Challenges

The most challenging aspect of this lab was configuring the storage within Linux.

Device naming conventions such as `/dev/sdb` and `/dev/xvdb` required careful attention, as AWS and Linux may reference the same storage device differently.

Understanding the purpose of `/etc/fstab` also required additional troubleshooting. Initially, it was unclear why the mounted storage would disappear after reboot, but configuring the correct mount entry resolved the issue and highlighted the importance of persistent filesystem configuration.

---

# Key Takeaways

* Amazon EBS provides persistent block storage for EC2 instances.
* Storage must be formatted and mounted before it can be used.
* `/etc/fstab` ensures storage persists across system reboots.
* AWS Systems Manager provides secure remote administration without SSH.
* EBS Snapshots enable efficient backup and disaster recovery.
* Cloud storage management requires both AWS infrastructure knowledge and Linux system administration skills.
