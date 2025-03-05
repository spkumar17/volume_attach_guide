# AWS EBS Volume Resizing Guide

This guide explains the process of safely increasing the size of an EBS volume attached to an EC2 instance.

## Prerequisites
- AWS Console access
- EC2 instance with an EBS volume
- SSH access to the EC2 instance

## Step-by-Step Process

### 1. Create a Snapshot (Backup)
Before resizing, create a snapshot of your EBS volume through the AWS Console for safety.

### 2. Resize the EBS Volume
1. Navigate to EC2 > Volumes in AWS Console
2. Select the volume
3. Choose Actions > Modify Volume
4. Enter the new size
5. Confirm the change

### 3. Extend the File System

First, check your current disk usage and file system type:
```bash
df -hT
```

Verify if the volume has a partition:
```bash
sudo lsblk
```

Extend the partition (if the volume uses partitions):
```bash
sudo growpart /dev/xvda 1
```

Extend the file system based on its type:

For XFS:
```bash
sudo xfs_growfs -d /
```

For Ext4:
```bash
sudo resize2fs /dev/xvda1
```

### Verification
Verify the new size is recognized:
```bash
df -h
```

## Troubleshooting
- If `growpart` fails, ensure the tool is installed: `sudo yum install -y growpart`
- For XFS systems, use `xfs_growfs`
- For Ext4 systems, use `resize2fs`
