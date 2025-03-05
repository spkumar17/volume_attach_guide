# AWS EBS New Volume Attachment Guide

This guide explains how to attach and configure a new EBS volume to an EC2 instance.

## Prerequisites
- AWS Console access
- EC2 instance running
- New EBS volume created
- SSH access to the EC2 instance

## Step-by-Step Process

### 1. Attach Volume in AWS Console
1. Create or select an EBS volume
2. Choose Actions > Attach Volume
3. Select target EC2 instance
4. Note the device name (e.g., /dev/xvdf)

### 2. Configure Volume on EC2 Instance

Check available block devices:
```bash
lsblk
```

Verify if the volume has a file system:
```bash
sudo file -s /dev/xvdf
```

If the output shows "data", create a file system:
```bash
sudo mkfs -t xfs /dev/xvdf
```

### 3. Mount the Volume

Create a mount point:
```bash
sudo mkdir -p /apps/my-data/apps/volume/new-volume
cd /apps/my-data/
```

Mount the volume:
```bash
sudo mount /dev/xvdf /apps/my-data/apps/volume/new-volume
```

### 4. Make the Mount Persistent (Optional)
To ensure the volume is mounted after system reboot, add an entry to `/etc/fstab`:
```bash
echo '/dev/xvdf /apps/my-data/apps/volume/new-volume xfs defaults 0 2' | sudo tee -a /etc/fstab
```

### Verification
Verify the mount:
```bash
df -h
```

## Best Practices
- Always label your volumes for easy identification
- Use consistent mount points
- Document volume purposes and configurations
- Regularly backup data using snapshots
