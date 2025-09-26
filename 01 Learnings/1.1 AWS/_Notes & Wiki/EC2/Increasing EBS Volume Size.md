---
tags:
  - AWS
  - EBS
---
---
### Steps to increase the EBS volume size
1. Go to the particular volume in EC2 console.
2. Select the volume.
3. Under Actions select the modify volume.
4. Enter the total size post increase.
5. Login to the instance
6. check if the size is being reflected using lsblk and df -Th
7. Run the following commands
```sh
growpart /dev/xvda 1
#Your block device and followed by its partition number. This will add the increased size to the mentioned partition.

Resize2fs /dev/xvda1
#To resize the file system for ext4

xfs_growfs /
#For xfs filesystem. Note: xfs_growfs works on the **mount point (like `/`), not the device name.
```