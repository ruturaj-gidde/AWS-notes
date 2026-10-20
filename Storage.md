# AWS Storage Services — Complete Interview Question Bank

For a Cloud/DevOps Engineer with 3–5 years of experience, you should understand not only what each storage service does, but also how to choose the right service, configure it securely, troubleshoot issues, and use it in production.

We'll cover these four services:

[Amazon S3 Simple Storage Service Logo Vector Png | **AWS** S3 Bucket Icon  - freepng](https://images.openai.com/static-rsc-4/rB8GMmjRGUBIfO9GRNEL6l37e3jVW1hoDOCdtBff_Wr7VxkdCHtrta2g8EOf4zncP-U_xh6GGgSiEkpH5sASXFUOS_FyAJByqom1RHN_8AH3G2_0e-dOIH9z8H7GWykzTw3-woJ0v9yGGL6R4soSj66bGyOOPAMJOgN_sNVgnU0?purpose=inline)

1\. Amazon S3 — Object storage

Store images, application files, logs, backups, static website assets, and Terraform state files.

[**EBS** (Elastic Block Store) | Cloud | **AWS** | Tech-Notes | difranca](https://images.openai.com/static-rsc-4/5cfY49yueKb0WMGEM21KwomMEvQP0Kc1SSH0FEXPhW_3t5PAh7_XRrYudGfnOxAQIfmlwGpvx-dOA6eZW801hYq-EfL4NpFRGcBLIRs1M-OBLr2zLfBfH5jhtNB6QYVhpooSBMPeWh-S5WlYy_44OjkcD89FRXUVaaETmFU1UQk?purpose=inline)

2\. Amazon **EBS** — Block storage

Persistent disk volumes for **EC2** instances, operating systems, databases, and application data.

[Amazon Elastic File System Logo **PNG** Vector (**SVG**) Free Download](https://images.openai.com/static-rsc-4/GzZ2wg3nmA65O1E2CaT-A9BmJ1cqi1qu0_CE3uWuyDLYI2S24avBdm4M74l2Fq_fHBD6HsCeR2RjQDP4yQBL42YAKRgYr7NsjHo7G8gnwUg2T36BVmTrO7FiIe6xpQ36I2qhuvdlLxp9lK4f07q3WA62WTHHJ1aEsdVR0y2I4CU?purpose=inline)

3\. Amazon **EFS** — Shared file storage

A managed, elastic file system that multiple Linux **EC2** instances can access concurrently.

[\[**AWS**\] S3 Glacier에서 데이터 꺼내는 옵션 (Expedited | Standard | Bulk Retrieval) — 람다코드](https://images.openai.com/static-rsc-4/ACBE2GiGMWwjGvFlerUB0QaPuUru6YfoOSHkZWbyYElog5n2a022j4RMTkHS_UdyMowfkSYvoZSKU1yKrNng6MXPgPGTw9m5qRPxscQGAwu598Wm9Vwg-8fYmZ2dG0B38ByXKLyiOpcHl3xVQ9RihIloHlU-dO7Gn9DFxTo0zVs?purpose=inline)

4\. S3 Glacier — Archive storage

Long-term data retention for backups, compliance records, and files that are rarely accessed.

## 1. Amazon S3 — Interview questions and answers

### Basic concepts

Q1. What is Amazon S3?

Answer: Amazon S3 (Simple Storage Service) is an object storage service used to store and retrieve data over the network. We use it for images, documents, application logs, backups, static website files, and Terraform state.

S3 stores data as objects inside buckets.

Example:

- Bucket: `company-app-prod`
- Object: `images/profile.jpg`
- Object: `logs/application.log`

Each object contains the data, a key (its name/path), and metadata. S3 is not a traditional mounted disk like **EBS**.

Q2. What is an S3 bucket?

A bucket is a container for objects. Bucket names must be globally unique within the **AWS** partition, and a bucket belongs to a specific **AWS** Region.

Example: `my-company-backups-prod`.

We can configure bucket-level settings such as versioning, encryption, lifecycle rules, logging, and access controls.

Q3. What is an S3 object?

An object is the actual data stored in S3. It consists of:

- Object data — the file contents.
- Key — the object's unique name within its bucket.
- Metadata — information such as content type and creation-related details.
- Version ID — when versioning is enabled, an object version can have an identifier.

For example, `reports/**2026**/january.pdf` is a key. The slashes represent a logical folder-like naming structure; S3 is fundamentally object storage.

Q4. Is S3 regional or global?

S3 buckets are created in a specific Region, but bucket names are globally unique within the **AWS** partition. The bucket's objects are stored in that Region unless you configure features such as cross-Region replication.

For disaster recovery, we can replicate objects to another Region.

Q5. Is S3 highly available and durable?

Yes. Amazon S3 Standard is designed for 99.99% availability and 99.**999999999**% (11 nines) durability.

Durability means protecting stored data against loss. Availability means the service is accessible when needed. These are different measurements.

Q6. Can we store any type of file in S3?

Yes. S3 can store images, videos, PDFs, logs, backups, archives, and other file formats as objects. The application determines how those files are used.

### Storage types and architecture

Q7. What are the main S3 storage classes?

| Storage class                 | Typical use                                                       |
| ----------------------------- | ----------------------------------------------------------------- |
| S3 Standard                   | Frequently accessed data                                          |
| S3 Intelligent-Tiering        | Access patterns are unpredictable                                 |
| S3 Standard-IA                | Infrequently accessed data that needs rapid retrieval             |
| S3 One Zone-IA                | Infrequent access where single-AZ storage is acceptable           |
| S3 Glacier Instant Retrieval  | Archive data requiring millisecond access                         |
| S3 Glacier Flexible Retrieval | Archive data with retrieval delays of minutes to hours            |
| S3 Glacier Deep Archive       | Very long-term archives, usually with retrieval measured in hours |

Choose based on access frequency, retrieval time, resilience requirements, and cost. Minimum storage durations and retrieval charges may apply.

Q8. What is the difference between S3 and **EBS**?

S3 is object storage accessed through APIs, SDKs, or tools. **EBS** is block storage that provides a disk volume for an **EC2** instance.

Example: Store application images and backups in S3; use **EBS** for an **EC2** operating system or a database disk.

Q9. Can we mount S3 like a normal filesystem?

S3 does not natively behave like a traditional block disk or **POSIX** filesystem. Tools can expose S3 objects through a filesystem-like interface, but their semantics and performance differ from a normal filesystem.

If an application requires shared filesystem behavior, consider **EFS** or another suitable file storage service.

**Q10**. What is S3 strong consistency?

S3 provides strong read-after-write consistency for object PUTs and DELETEs. After a successful write or delete, subsequent reads and listings reflect the change.

This means applications generally do not need to wait for eventual consistency after a normal S3 object update.

### S3 security and encryption

**Q11**. How do you make an S3 bucket private?

Answer: I enable S3 Block Public Access at the account and bucket levels, avoid public bucket policies and ACLs, and grant access only to authorized **IAM** users or roles.

For an application running on **EC2**, I attach an **IAM** role with the required S3 permissions instead of storing access keys on the instance.

**Q12**. What is S3 Block Public Access?

It is a set of controls that helps prevent public access to S3 buckets and objects. It can block public bucket policies and public **ACL**-based access.

For production buckets, I normally enable all four Block Public Access settings unless there is a specific, reviewed public-access requirement.

**Q13**. What is the difference between an **IAM** policy and a bucket policy?

- **IAM** policy: Attached to a user, group, or role to define what that identity can do.
- Bucket policy: A resource-based policy attached to an S3 bucket to define who can access it and under what conditions.

Example: An **EC2** role has permission to upload files, while the bucket policy can further restrict access to approved principals or network paths. An explicit deny can override an allow.

**Q14**. What is **SSE**-S3?

**SSE**-S3 means server-side encryption with Amazon S3-managed keys. S3 automatically encrypts objects at rest and manages the encryption keys.

It is a good default for many applications because encryption is simple to enable and requires little key-management effort.

**Q15**. What is the difference between **SSE**-S3 and **SSE**-**KMS**?

| SSE-S3                               | SSE-KMS                                                                       |
| ------------------------------------ | ----------------------------------------------------------------------------- |
| S3 manages the encryption keys       | AWS KMS manages the keys                                                      |
| Simpler key management               | More control over key policies and permissions                                |
| No separate KMS key-management setup | Can use AWS-managed or customer-managed KMS keys                              |
| Suitable for many standard workloads | Useful when key-level control, auditability, or compliance requirements apply |

Both encrypt data at rest. **SSE**-**KMS** can introduce additional **KMS** costs and request quotas.

**Q16**. Does encryption make an S3 bucket private?

No. Encryption protects data at rest, but it does not automatically prevent unauthorized access.

I use encryption together with Block Public Access, least-privilege **IAM** policies, suitable bucket policies, and monitoring.

**Q17**. What is an S3 **VPC** endpoint?

A **VPC** endpoint lets resources in a **VPC** access supported **AWS** services without requiring traffic to traverse the public internet.

For S3, the two relevant endpoint types are:

- Gateway endpoint: Commonly used to access S3 from a **VPC** through route tables.
- Interface endpoint: Uses **AWS** PrivateLink and elastic network interfaces with private IP addresses.

For a typical **EC2** instance in a private subnet that needs S3 access, I would consider a gateway endpoint first.

**Q18**. How does a private **EC2** instance access S3 without a **NAT** Gateway?

The traffic flow is:

## Create an S3 gateway endpoint in the VPC.

## Associate the endpoint with the private subnet's route table. ## Attach an IAM role to the EC2 instance with the necessary S3 permissions. ## Ensure the bucket policy, endpoint policy, and any applicable KMS permissions allow the request. ## The instance accesses S3 through the endpoint without requiring a NAT Gateway for that S3 traffic.

### S3 versioning, replication, and lifecycle

**Q19**. What is S3 Versioning?

Versioning maintains multiple versions of an object in a bucket. If a user overwrites or deletes an object, earlier versions can often be recovered.

Example: Someone accidentally replaces `app-config.json`. With versioning enabled, we can retrieve the previous version.

Important: Versioning does not by itself protect against every kind of deletion or malicious activity. Restrict version-deletion permissions and consider S3 Object Lock where appropriate.

**Q20**. What happens when you delete a versioned S3 object?

A normal delete generally creates a delete marker, making the object appear deleted in ordinary listings and **GET** requests. Previous versions remain until they are explicitly deleted or removed by lifecycle rules.

If you permanently delete a specific version, that version cannot be recovered through S3 Versioning.

**Q21**. What is S3 Cross-Region Replication (**CRR**)?

**CRR** asynchronously replicates eligible objects from a source bucket to a destination bucket in a different **AWS** Region.

Common use cases include disaster recovery, regional data requirements, and maintaining another copy of important files.

Requirements include versioning on both buckets, appropriate replication permissions, and a replication configuration.

**Q22**. What is Same-Region Replication (**SRR**)?

**SRR** replicates eligible objects between buckets in the same **AWS** Region. It can help separate production data from another copy for access separation, log aggregation, or other operational requirements.

**SRR** and **CRR** are asynchronous, so they should not automatically be treated as a zero-data-loss backup strategy.

**Q23**. What is an S3 lifecycle policy?

A lifecycle configuration automates actions on objects as they age.

Example:

- Keep frequently accessed files in S3 Standard.
- Transition older logs to a lower-cost storage class.
- Expire temporary files after 30 days.
- Expire noncurrent versions after a suitable retention period.

Before applying lifecycle rules, check business retention requirements, minimum storage durations, and retrieval charges.

**Q24**. What is S3 Object Lock?

S3 Object Lock provides **WORM** (Write Once, Read Many) protection for objects. It can prevent protected object versions from being deleted or overwritten for a retention period or indefinitely under a legal hold.

It is useful for compliance records and immutable backups.

**Q25**. What is an S3 pre-signed **URL**?

A pre-signed **URL** grants temporary access to a particular S3 operation, such as downloading or uploading an object, without giving the recipient permanent **AWS** credentials.

Example: An application generates a **URL** that allows a customer to download an invoice for a limited period.

The **URL** acts like a temporary bearer credential, so it must be protected and given an appropriate expiration time.

### S3 production scenarios

**Q26**. An **EC2** instance cannot upload files to S3. How do you troubleshoot it?

I check the following in order:

## IAM role: Does the instance have an attached role?

## IAM permissions: Does it have the required `s3:PutObject` permission on the correct object ARN? ## Bucket policy: Is there an explicit deny or a principal restriction? ## Network: Does the instance have connectivity through a NAT Gateway or S3 VPC endpoint? ## Endpoint policy: If an endpoint is used, does its policy permit the request? ## Encryption: If SSE-KMS is used, are the necessary KMS permissions present? ## Logs and errors: Check the AWS CLI error, application logs, CloudTrail data events if configured, and relevant service metrics.

**Q27**. Someone accidentally deletes important S3 files. What do you do?

First, I stop any process that may continue deleting files. If versioning is enabled, I identify the deleted objects and recover the required previous versions or remove delete markers where appropriate.

Then I investigate the event using available audit logs, review the identity and permissions involved, and implement preventive controls such as least-privilege policies, restricted version deletion, lifecycle review, and Object Lock if required.

**Q28**. How do you reduce S3 storage costs?

I analyze object sizes, age, and access patterns. Then I use lifecycle policies, suitable storage classes, expiration of unnecessary data, and intelligent tiering where appropriate.

I also review incomplete multipart uploads, old object versions, retrieval charges, and replication costs before making changes.

**Q29**. Your company wants a static website hosted on S3. What architecture would you use?

For a production website, I would typically use:

- S3 to store **HTML**, **CSS**, JavaScript, and static assets.
- CloudFront as the content delivery network.
- Origin Access Control (**OAC**) to let CloudFront access a private S3 bucket.
- **AWS** **WAF** if web-application filtering is required.
- Route 53 and an **ACM** certificate for a custom domain and **HTTPS**.

The bucket stays private; users access the website through CloudFront. The browser can still see the **HTML** and JavaScript delivered to it, so frontend code must never contain secrets.

**Q30**. How do you monitor S3?

I use CloudWatch metrics for storage requests and errors, S3 Storage Lens for storage-usage insights, CloudTrail for supported **API** activity, and server access logging when detailed request logs are needed.

For important production buckets, I also monitor unexpected access patterns and review configuration changes.

### Terraform example: Create an encrypted, private S3 bucket

``` resource *aws_s3_bucket* *app* { bucket = *my-company-app-prod-unique* }

resource *aws_s3_bucket_public_access_block* *app* { bucket = aws_s3_bucket.app.id

    block_public_acls       = true
    ignore_public_acls      = true
    block_public_policy     = true
    restrict_public_buckets = true
}

resource *aws_s3_bucket_versioning* *app* { bucket = aws_s3_bucket.app.id

    versioning_configuration {
    status = *Enabled*
    }
}

resource *aws_s3_bucket_server_side_encryption_configuration* *app* { bucket = aws_s3_bucket.app.id

    rule {
    apply_server_side_encryption_by_default {
    sse_algorithm = *AES256*
    }
    }
}
```

This enables **SSE**-S3, versioning, and public-access blocking. The bucket name must be unique, and production deployments should also use appropriate **IAM** access controls and review any existing bucket policies.

## 2. Amazon EBS — Interview questions and answers

### Basic concepts

**Q31**. What is Amazon **EBS**?

Amazon Elastic Block Store (**EBS**) provides persistent block-storage volumes for **EC2** instances.

Think of **EBS** as an additional hard disk attached to a server. You can use it for the operating system, application files, databases, and other data that needs disk-level access.

Example: An **EC2** instance running PostgreSQL can use an **EBS** volume to store database files.

**Q32**. What is an **EBS** volume?

An **EBS** volume is a virtual block device that provides storage capacity to an **EC2** instance.

You can create, attach, detach, resize, and snapshot volumes. After attaching a new volume to Linux, you may need to partition it, format it, and mount it before use.

**Q33**. What happens to **EBS** data when an **EC2** instance is terminated?

It depends on the volume's `DeleteOnTermination` setting.

- Root volume: Often deleted when the instance is terminated, depending on configuration.
- Additional data volume: Often retained if configured not to delete on termination.

Always verify the setting before terminating a production instance.

**Q34**. What is the difference between an **EBS** volume and an **EBS** snapshot?

An **EBS** volume is active block storage attached to an instance. A snapshot is a point-in-time backup of a volume, stored and managed by **AWS**.

Example: We use an **EBS** volume for a running database and take snapshots for backup and recovery.

**Q35**. Can an **EBS** volume be attached to multiple **EC2** instances?

Normally, an **EBS** volume is attached to one **EC2** instance at a time. Certain **EBS** volume types support Multi-Attach, allowing a volume to be attached to multiple compatible **EC2** instances within the same Availability Zone.

Multi-Attach requires a supported configuration and application or cluster-level coordination to prevent data corruption. It is not equivalent to a shared filesystem like **EFS**.

**Q36**. Is **EBS** regional or zonal?

An **EBS** volume is a zonal resource. It is associated with one Availability Zone and can be attached only to compatible instances in that same AZ.

For cross-AZ recovery, create a snapshot and restore a volume in another AZ, or use an appropriate replication or recovery solution.

**Q37**. What are the main **EBS** volume types?

| Volume type | Typical use                                                       |
| ----------- | ----------------------------------------------------------------- |
| `gp3`       | General-purpose SSD for most applications                         |
| `gp2`       | Older general-purpose SSD workloads                               |
| `io2`       | High-performance, high-IOPS workloads such as demanding databases |
| `io1`       | Older provisioned-IOPS SSD workloads                              |
| `st1`       | Throughput-intensive HDD workloads                                |
| `sc1`       | Infrequently accessed, throughput-oriented HDD workloads          |

For many new general-purpose workloads, `gp3` is a sensible starting point. Choose based on **IOPS**, throughput, latency, capacity, and cost requirements.

**Q38**. What are **IOPS** and throughput?

- **IOPS**: Number of input/output operations completed per second.
- Throughput: Amount of data transferred per second, typically measured in MiB/s or MB/s.

A database may need high **IOPS** for many small operations. A workload processing large files may benefit more from high throughput.

**Q39**. Can we increase **EBS** volume size without stopping **EC2**?

In many cases, yes. **EBS** supports Elastic Volumes for supported configurations, allowing you to modify volume size and certain performance settings while the volume is in use.

After increasing the volume, you may also need to expand the partition and filesystem inside Linux. Take appropriate backups and verify filesystem support before changing a production volume.

### EBS security and recovery

**Q40**. How do you encrypt an **EBS** volume?

**EBS** supports encryption at rest using **AWS** **KMS** keys. Encryption protects the volume's data and supports encryption of associated snapshots and data transferred between supported infrastructure components.

For production workloads, I enable encryption by default and use a suitable **KMS** key according to security and compliance requirements.

**Q41**. What happens if you create an encrypted **EBS** snapshot?

The snapshot remains encrypted. Volumes created from that encrypted snapshot are also encrypted, using the applicable key configuration.

When copying snapshots between Regions or accounts, verify key permissions and the encryption settings for the destination.

**Q42**. How do you back up an **EC2** server?

I use **EBS** snapshots for its **EBS** volumes or **AWS** Backup where centralized backup management is appropriate.

For databases, I also consider database-native backups, application consistency, and recovery testing. A snapshot alone may not guarantee application-consistent recovery for every workload.

**Q43**. What is an **EBS** snapshot? Is it a full or incremental backup?

**EBS** snapshots are managed point-in-time backups. Subsequent snapshots store changed data blocks efficiently rather than independently duplicating all previously backed-up data.

**AWS** manages the underlying storage and snapshot mechanics. I retain the required recovery points and test restores regularly.

**Q44**. What is the difference between **EBS** snapshots and AMIs?

- **EBS** snapshot: Backs up a volume.
- **AMI** (Amazon Machine Image): Provides the information needed to launch an **EC2** instance, including its boot configuration and associated image data.

An **EBS**-backed **AMI** uses snapshots of its relevant **EBS** volumes. An **AMI** is useful when you want to launch new instances with a prepared operating system and software.

### EBS production troubleshooting

**Q45**. An **EC2** instance is running out of disk space. What do you do?

I first identify which filesystem is full.

``` df -h df -i lsblk sudo du -xhd1 /var 2>/dev/null | sort -h ```

- `df -h` checks filesystem capacity.
- `df -i` checks inode usage.
- `lsblk` shows disks and partitions.
- `du` helps locate large directories.

Then I inspect logs, temporary files, and application data. I clean only files confirmed to be safe to remove. If more capacity is needed, I expand the **EBS** volume, extend the partition if applicable, and grow the filesystem.

**Q46**. You increased the **EBS** volume from 20 GiB to 50 GiB, but Linux still shows 20 GiB. Why?

Increasing the **EBS** volume does not always automatically expand the partition and filesystem.

I verify the volume size using `lsblk`, then extend the partition if required and expand the filesystem with the correct tool. For example, `resize2fs` is used for ext4 and `xfs_growfs` for **XFS**.

**Q47**. How do you troubleshoot slow **EBS** performance?

I check CloudWatch metrics such as volume read/write latency, queue length, **IOPS**, throughput, and burst-balance metrics where applicable. I also inspect **EC2** instance-level **EBS** bandwidth limits and the application's disk I/O.

Then I determine whether the bottleneck is volume capacity, **IOPS**, throughput, instance limits, or the application itself. I adjust the volume type or performance settings only after identifying the bottleneck.

**Q48**. An **EC2** instance has a failed root **EBS** volume. What is your recovery approach?

I investigate the volume and instance status checks, console output, and relevant logs. If recovery is needed, I may stop the instance, detach the root volume where appropriate, attach it to a recovery instance, inspect or repair the filesystem, and restore from a known-good snapshot if necessary.

I preserve the original volume or snapshot when possible and validate the recovery before returning the application to service.

**Q49**. How do you achieve high availability with **EBS**?

**EBS** volumes themselves are zonal. To improve application availability, deploy application instances across multiple AZs and use appropriate data replication or backup strategies.

For a database, use a database-supported replication or managed database solution, rather than assuming an **EBS** volume can simply be shared across AZs.

### Terraform example: Create an EBS volume

```
resource *aws_ebs_volume* *data* {
    availability_zone = *ap-southeast-2a*
    size              = 20
    type              = *gp3*
    encrypted         = true

    tags = {
    Name = *app-data*
    }
}

resource *aws_volume_attachment* *data* {
    device_name = */dev/sdf*
    volume_id   = aws_ebs_volume.data.id
    instance_id = aws_instance.app.id
}
```

Here, `aws_instance.app.id` assumes you already have an **EC2** resource named `app`. The volume and instance must be in the same AZ. On Linux, attachment alone does not format or mount the volume.

## 3. Amazon EFS — Interview questions and answers

### Basic concepts

**Q50**. What is Amazon **EFS**?

Amazon Elastic File System is a managed file storage service that provides a shared filesystem for multiple supported clients.

Example: Two or more Linux **EC2** instances running an application can mount the same **EFS** filesystem and access shared files.

**Q51**. Why do we use **EFS** instead of **EBS**?

**EFS** is useful when multiple instances need to access the same files concurrently. **EBS** is typically used when an instance needs its own block-storage volume.

Example: Multiple web servers may need access to shared uploads. **EFS** can provide a common filesystem, whereas separate **EBS** volumes would not automatically share files between those servers.

**Q52**. How does **EFS** work?

## Create an EFS filesystem.

## Configure mount targets in the required VPC Availability Zones. ## Configure network connectivity and security groups. ## Install or use the appropriate NFS client on the EC2 instance. ## Mount the filesystem. ## Applications read and write files through the mounted path.

**EFS** supports the **NFS** protocol; Linux workloads commonly use NFSv4.1.

**Q53**. What is an **EFS** mount target?

A mount target provides network access to an **EFS** filesystem from a **VPC**. It is associated with a subnet and has a network interface and private IP address.

For multi-AZ workloads, we generally create mount targets in the AZs where clients run, subject to **EFS**'s subnet and Availability Zone requirements.

**Q54**. Can multiple **EC2** instances access **EFS** simultaneously?

Yes. Multiple authorized clients can mount the same **EFS** filesystem and access shared data concurrently.

The application must still handle concurrent writes, file locking, and data consistency correctly where needed. Shared storage does not automatically make an application safe for concurrent access.

**Q55**. Is **EFS** regional or zonal?

**EFS** Regional storage is designed for multi-AZ availability within a Region. **EFS** One Zone stores filesystem data in one AZ and may suit workloads that accept its different resilience characteristics.

This differs from **EBS**, which is a zonal volume.

**Q56**. Does **EFS** automatically increase its storage capacity?

**EFS** storage capacity scales elastically as files are added or removed, without requiring you to manually resize a filesystem in the same way as an **EBS** volume.

Performance and throughput characteristics depend on the **EFS** performance and throughput modes, storage class, and workload.

### EFS security and performance

**Q57**. How do you secure **EFS**?

I use a dedicated security group for **EFS** mount targets and allow inbound **TCP** port **2049** (**NFS**) only from the authorized client security groups.

I also configure **IAM** authorization when appropriate, **EFS** access points for controlled application access, encryption at rest, and **TLS** in transit.

**Q58**. What is an **EFS** access point?

An **EFS** access point provides an application-specific entry point into a filesystem. It can enforce a root directory and **POSIX** user/group identity for connections that use it.

For example, different applications can use separate access points to access designated directories with controlled identities.

**Q59**. What are **EFS** performance modes?

**EFS** offers General Purpose and Max I/O performance modes, subject to current service availability and configuration.

General Purpose is the usual choice for latency-sensitive applications. Max I/O is designed for certain highly parallelized workloads where higher aggregate scaling may be more important than latency.

Check current **AWS** support and mode-selection constraints before choosing a mode for a new filesystem.

**Q60**. What are **EFS** throughput modes?

**EFS** provides throughput options such as Elastic, Provisioned, and Bursting throughput, depending on filesystem configuration and current service capabilities.

- Elastic: Automatically adjusts throughput to the workload.
- Provisioned: Lets you specify throughput independently of the amount of stored data.
- Bursting: Throughput depends on the storage amount and available burst credits.

The best option depends on the traffic pattern, workload, and cost.

**Q61**. What is the difference between **EFS** Standard and **EFS** Infrequent Access?

**EFS** Standard is intended for frequently accessed files. **EFS** Infrequent Access (IA) provides a lower storage price for less frequently accessed data, generally with additional access costs.

Lifecycle management can move files between suitable storage classes based on access patterns. Evaluate access charges and latency requirements before using IA.

### EFS production troubleshooting

**Q62**. An **EC2** instance cannot mount **EFS**. What do you check?

## Confirm the EFS filesystem is available.

## Check that a mount target exists in a suitable AZ/subnet. ## Verify network connectivity and security-group rules for TCP 2049. ## Check the NFS client and DNS resolution. ## Review mount command output and system logs. ## Verify IAM authorization or access-point configuration if enabled.

A frequent cause is that the **EFS** security group allows no inbound **NFS** traffic from the **EC2** security group.

**Q63**. Two **EC2** instances mount the same **EFS**. One application cannot write files. Why?

Possible causes include **POSIX** ownership, file permissions, **UID**/**GID** mismatch, a read-only mount, an **EFS** access point enforcing a different identity, or **IAM** authorization issues.

I inspect the mount options, user identity, file permissions, and logs before changing permissions.

**Q64**. **EFS** is slow. How do you troubleshoot it?

I examine CloudWatch metrics for throughput, latency, client connections, and burst credits where applicable. I review the selected throughput and performance modes, storage-class access behavior, client network conditions, and application I/O patterns.

Then I optimize the workload or adjust the configuration based on the actual bottleneck.

**Q65**. When would you use **EFS** in a production project?

A typical use case is a web application running on multiple **EC2** instances where every instance needs access to shared uploads or documents.

I would use **EFS** when shared filesystem semantics are required, while keeping static assets and downloadable objects in S3 when object storage is more suitable.

### Terraform example: Create EFS

``` resource *aws_efs_file_system* *app* { encrypted = true

    tags = {
    Name = *app-shared-files*
    }
}

resource *aws_efs_mount_target* *app* {
    file_system_id  = aws_efs_file_system.app.id
    subnet_id       = aws_subnet.private.id
    security_groups = [aws_security_group.efs.id]
}
```

This example assumes the private subnet and **EFS** security group already exist. The security group should allow **TCP** **2049** only from the intended client security group. For multiple AZs, configure suitable mount targets for each client AZ.

## 4. Amazon S3 Glacier — Interview questions and answers

### Basic concepts

**Q66**. What is S3 Glacier?

S3 Glacier is a set of S3 storage classes designed for archiving data at lower storage costs.

It is useful for long-term backups, audit records, compliance data, and historical files that are rarely accessed.

**Q67**. Is S3 Glacier a separate service or an S3 storage class?

Amazon S3 Glacier storage classes are part of Amazon S3. We can store objects in these classes directly or transition eligible objects using lifecycle rules.

Glacier is not the same as an **EBS** snapshot or an **EFS** filesystem.

**Q68**. What is the difference between Glacier Instant Retrieval, Flexible Retrieval, and Deep Archive?

| Feature            | Instant Retrieval                          | Flexible Retrieval                          | Deep Archive                                                  |
| ------------------ | ------------------------------------------ | ------------------------------------------- | ------------------------------------------------------------- |
| Access pattern     | Rarely accessed                            | Rarely accessed                             | Very rarely accessed                                          |
| Retrieval          | Milliseconds                               | Minutes to hours, depending on option       | Usually hours                                                 |
| Typical use        | Archive with immediate access requirements | Long-term backups                           | Long-term compliance archives                                 |
| Cost consideration | Higher storage cost than deeper archives   | Lower storage cost, retrieval charges apply | Typically the lowest storage cost, with longer retrieval time |

The exact retrieval time depends on the retrieval option and service conditions. Always check current **AWS** pricing and retention requirements.

**Q69**. How do you retrieve an object stored in S3 Glacier Flexible Retrieval?

For an archived object, you generally initiate a restore request, choose an appropriate retrieval tier, and wait for the restored copy to become available.

After restoration completes, the object can be downloaded during the configured restoration period. The original archived object remains in its archival storage class.

For S3 Glacier Instant Retrieval, objects can be accessed directly without a separate restore request.

**Q70**. What is the difference between S3 Glacier and S3 Standard?

S3 Standard is designed for frequently accessed data with immediate access. Glacier storage classes are designed for archival use, often trading retrieval time or retrieval costs for lower storage costs.

Example: Keep active application logs in S3 Standard and transition older logs to an appropriate archival class.

**Q71**. How can you move S3 objects to Glacier automatically?

I configure an S3 lifecycle rule based on object age, prefixes, tags, or other supported filters.

For example, move eligible logs to Glacier Flexible Retrieval after 90 days and expire them after the approved retention period.

I confirm compliance requirements and cost implications before setting expiration rules.

**Q72**. Can we use S3 Versioning with Glacier storage classes?

Yes. Versioned objects can use lifecycle transitions, including transitions for noncurrent versions where supported.

A lifecycle policy should account for both current and noncurrent versions; otherwise, old versions may continue consuming storage and incurring charges.

**Q73**. What happens if you delete a Glacier object?

The outcome depends on the storage class, versioning configuration, and lifecycle or retention controls.

In a versioned bucket, a normal delete can create a delete marker while leaving older versions stored. Object Lock can prevent protected versions from being deleted during the applicable retention period.

Before deleting archived records, check retention obligations and the effects on existing versions.

**Q74**. When should you not use Glacier?

I would avoid a deep archival class for data that must be accessed immediately or frequently. Restore delays and retrieval charges may make it unsuitable for live application data, active websites, or frequently queried files.

### Glacier production scenarios

**Q75**. A business wants to keep backups for seven years at a low cost. What would you recommend?

I would evaluate S3 Glacier Deep Archive for backups that rarely need retrieval, using lifecycle policies and an approved retention schedule.

I would also consider backup integrity, encryption, access restrictions, cross-account protection, Object Lock where required, and periodic restore testing.

**Q76**. A customer needs an archived file immediately. What do you do?

First, I check which S3 storage class contains the object.

If it is in Glacier Instant Retrieval, direct access is supported. If it is in Flexible Retrieval or Deep Archive, I initiate a restore using a retrieval option that meets the customer's needs, then download the file once it is available.

For urgent recovery, I explain the expected retrieval time and any applicable charges rather than promising immediate access.

**Q77**. How do you reduce archival storage costs?

I identify old data, define retention and deletion requirements, and configure lifecycle transitions to suitable archival classes. I also review noncurrent versions, incomplete multipart uploads, retrieval frequency, and minimum storage-duration charges.

I test the lifecycle policy on a small set of objects before applying it broadly.

## 5. Comparison and tricky interview questions

These questions are especially important because interviewers often give a scenario rather than ask for a definition.

**Q78**. Explain S3 vs **EBS** vs **EFS** vs Glacier.

| Requirement                                            | Best starting choice     |
| ------------------------------------------------------ | ------------------------ |
| Store images, documents, logs, or downloadable objects | S3                       |
| Provide a disk for an EC2 server                       | EBS                      |
| Share files across multiple EC2 instances              | EFS                      |
| Store rarely accessed data for long-term archival      | S3 Glacier storage class |

**Q79**. An application running on 10 **EC2** instances needs to share uploaded files. Which service do you choose?

If the application requires a shared filesystem, I would consider **EFS**. All authorized instances can mount the same filesystem and access shared files.

If the application can access files through object APIs, S3 may be a better fit. The decision depends on whether filesystem semantics are necessary.

**Q80**. A database is running on **EC2**. Where would you store its data?

For a database installed directly on **EC2**, I would normally use **EBS** volumes sized and configured for the database's **IOPS**, throughput, latency, and capacity requirements.

I would also configure backups, encryption, monitoring, and an appropriate recovery strategy. If using Amazon **RDS** instead, **AWS** manages much of the underlying database storage infrastructure.

**Q81**. You need to serve images to millions of users. Which storage service do you use?

I would generally store images in S3 and serve them through CloudFront. CloudFront caches content at edge locations, reducing latency and repeated requests to the origin.

For private content, I would configure access controls and use an appropriate signed **URL** or signed-cookie strategy.

**Q82**. A company requires a seven-year backup retention period. Should you keep all backups in S3 Standard?

Not necessarily. I would use lifecycle policies to transition eligible backups to more cost-effective storage classes based on access requirements.

For rarely accessed data, Glacier classes may be appropriate. I would also enforce the approved retention period and verify that recovery requirements are met.

**Q83**. Can **EBS** replace S3?

No, not directly. **EBS** provides block storage for servers, while S3 provides object storage accessed through APIs.

A mounted **EBS** disk and an S3 bucket have different access patterns, scaling behavior, and application interfaces.

**Q84**. Can **EFS** replace S3?

Not in every situation. **EFS** provides shared filesystem access, while S3 is designed for object storage.

Use **EFS** for applications that need shared file operations and S3 for objects, backups, static assets, and large collections of files accessed through APIs.

**Q85**. What is the difference between **EBS** snapshots and S3 backups?

**EBS** snapshots back up **EBS** volume data. S3 backups may contain application files, exported database backups, logs, or other objects.

For disaster recovery, the backup should be appropriate to the system being protected. For example, backing up a database file to S3 is not necessarily sufficient unless the backup process produces a recoverable, consistent database backup.

**Q86**. How would you design a secure, cost-effective storage architecture for a production application?

A reasonable starting design would be:

- S3 for uploads, static assets, logs, and backup objects.
- CloudFront for cached content delivery where useful.
- **EBS** for **EC2** operating systems and application-specific block storage.
- **EFS** only for workloads requiring shared filesystem access.
- Glacier storage classes for older data with suitable retrieval requirements.
- Encryption, least-privilege **IAM**, private networking where appropriate, versioning or immutable backups, monitoring, and lifecycle rules across the relevant resources.

I would validate the design against the application's performance, recovery, compliance, and cost requirements.

## 6. More production scenarios interviewers ask

**Q87**. An S3 bucket contains confidential customer information. How do you protect it?

I enable Block Public Access, use least-privilege **IAM** permissions, encrypt objects at rest, and require **HTTPS** for data in transit. I review bucket policies and use CloudTrail data events where appropriate to audit object-level access.

For sensitive data, I also consider **KMS** key policies, data classification, access monitoring, and retention requirements.

**Q88**. How do you copy data from one S3 bucket to another **AWS** account?

I configure the destination bucket policy to authorize the required source identity, or use an appropriate cross-account role. I also check encryption and **KMS** permissions, especially if the source objects use **SSE**-**KMS**.

For recurring replication, I configure S3 Cross-Region or Same-Region Replication as appropriate. For a one-time copy, I can use **AWS** **CLI** commands such as `aws s3 sync`, provided permissions and object requirements are satisfied.

**Q89**. An **EC2** instance can access S3, but it cannot download an **SSE**-**KMS** encrypted object. Why?

The instance might have S3 permissions but lack permission to use the **KMS** key. I check `s3:GetObject`, applicable **KMS** permissions such as `kms:Decrypt`, the key policy, and any explicit denies.

S3 authorization and **KMS** authorization are separate checks.

**Q90**. What is the difference between S3 durability and availability?

Durability refers to the likelihood of preserving data without loss. Availability refers to whether the service is accessible when requested.

A service can be highly durable but still experience a temporary availability issue.

**Q91**. How do you make an **EBS**-backed application recoverable after an AZ failure?

I would use a multi-AZ application design and a recovery strategy appropriate to the workload. **EBS** volumes cannot simply be attached across AZs, so I would restore snapshots into the target AZ or use database replication and other suitable recovery mechanisms.

I would also test failover and recovery time rather than relying solely on backups.

**Q92**. A private **EC2** instance needs to access S3, but the company does not want a **NAT** Gateway. What do you do?

I configure an S3 gateway **VPC** endpoint and associate it with the relevant private route tables. I then verify **IAM** permissions, endpoint policies, bucket policies, and **KMS** permissions if applicable.

This allows S3 access without requiring a **NAT** Gateway for that traffic.

**Q93**. What is the difference between a backup and replication?

A backup provides a recoverable copy at a point in time. Replication maintains copies of data in another location or bucket, often asynchronously.

Replication can also replicate accidental changes or deletions depending on configuration. Therefore, replication alone is not a complete backup strategy.

**Q94**. What would you check before deleting an old S3 object or **EBS** volume?

I verify ownership, application dependencies, retention requirements, backups, and whether the resource is actively used. For S3, I check object versions, lifecycle rules, and Object Lock. For **EBS**, I check attachments, mount points, and recovery requirements.

I follow the change-management process and take a recovery point when needed before destructive changes.

**Q95**. How do you monitor storage costs in **AWS**?

I use **AWS** Cost Explorer, **AWS** Budgets, and cost-allocation tags to identify cost trends and responsible workloads. For S3, I review Storage Lens, storage-class distribution, old versions, and retrieval costs. For **EBS**, I review unused volumes, oversized volumes, and snapshots.

I remove resources only after confirming that they are no longer needed.

## 7. Useful AWS CLI commands

These are good practical commands to remember for interviews and troubleshooting.

| Task                    | Command                                       |
| ----------------------- | --------------------------------------------- |
| List S3 buckets         | `aws s3 ls`                                   |
| List files in a bucket  | `aws s3 ls s3://my-bucket/`                   |
| Upload a file           | `aws s3 cp file.txt s3://my-bucket/`          |
| Download a file         | `aws s3 cp s3://my-bucket/file.txt .`         |
| Synchronize directories | `aws s3 sync ./data s3://my-bucket/data/`     |
| Check EC2 volumes       | `aws ec2 describe-volumes`                    |
| Check EBS snapshots     | `aws ec2 describe-snapshots --owner-ids self` |
| Check EFS filesystems   | `aws efs describe-file-systems`               |

For Linux disk troubleshooting:

``` df -h df -i lsblk du -sh /var/log/* ```

These commands help identify filesystem capacity, inode exhaustion, attached block devices, and large log directories.

## 8. Quick revision test

## Test your understanding

0/6 answered

1\. Which service is best for storing application images and backups?

**EBS**

S3

**EFS**

2\. Which service provides block storage for an **EC2** database?

### Glacier Deep Archive

**EFS**

**EBS**

3\. Three Linux **EC2** instances must access the same filesystem. Which service would you consider?

**EFS**

**EBS** snapshots

S3 Glacier

4\. Which storage class is a good candidate for rarely accessed, long-term archives?

S3 Standard

S3 Glacier Deep Archive

**EBS** gp3

5\. An **EC2** instance has S3 permissions but cannot read an **SSE**-**KMS** object. What else might be missing?

An **EFS** mount target

**KMS** decrypt permission

An **EBS** snapshot

6\. What is the best first check when Linux reports no free disk space?

df -h and df -i

Create an S3 bucket

Change the **EC2** instance name

Check answers

Interview priority: Focus first on S3 security, versioning, replication, lifecycle, and **VPC** endpoints; **EBS** volume expansion and disk-full troubleshooting; **EFS** networking and permissions; and Glacier retrieval and retention. These topics help demonstrate practical production experience rather than just memorized definitions.
