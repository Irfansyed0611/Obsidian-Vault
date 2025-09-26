---
tags:
  - AWS
  - EC2
  - AMI
---
Amazon Machine Images (AMIs) in AWS are essentially templates used to launch new EC2 instances. When you create an AMI from an EC2 instance—especially using AWS Backup—the AMI is backed by Amazon EBS snapshots. Here’s how storage and billing work for AMIs:

## How AMI Storage Works

- **AMI Storage**: Each AMI created from an EBS-backed instance is stored as one or more EBS snapshots, representing the data on the instance’s volumes at the time the AMI was created[4](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/AMIs.html)[6](https://serverfault.com/questions/227929/approximately-how-much-would-it-cost-to-make-an-ami-image-from-a-ec2-micro-serve).
    
- **Incremental Snapshots**: EBS snapshots are incremental. This means only the blocks that have changed since the last snapshot are stored as new data; unchanged blocks are referenced from previous snapshots[3](https://stackoverflow.com/questions/55989633/aws-ec2-get-the-ami-size).
    
- **AMI Size**: The space consumed by each AMI is not simply the total size of the volumes but rather the size of the new or changed data blocks since the last snapshot[3](https://stackoverflow.com/questions/55989633/aws-ec2-get-the-ami-size)[6](https://serverfault.com/questions/227929/approximately-how-much-would-it-cost-to-make-an-ami-image-from-a-ec2-micro-serve). If you create multiple AMIs in sequence with minimal changes between them, each new AMI will consume only the storage required for the changes.

## AWS Charging Model for AMIs

- **Snapshot Storage Cost**: You are billed for the storage consumed by the EBS snapshots that back your AMIs. As of recent pricing, EBS snapshot storage costs approximately $0.05 per GB per month (check the AWS region for exact pricing)[6](https://serverfault.com/questions/227929/approximately-how-much-would-it-cost-to-make-an-ami-image-from-a-ec2-micro-serve).
    
- **Incremental Charges**: Since snapshots are incremental, you are only charged for the unique blocks stored in each snapshot. If 20 AMIs are created and there is little change between each, most of the data is shared, and only the differences consume additional storage and incur charges[3](https://stackoverflow.com/questions/55989633/aws-ec2-get-the-ami-size)[6](https://serverfault.com/questions/227929/approximately-how-much-would-it-cost-to-make-an-ami-image-from-a-ec2-micro-serve).
    
- **Example Calculation**: If your instance uses 5 GB of data and you create one AMI, you are charged for 5 GB of snapshot storage (~$0.25/month). If you create 20 AMIs with little change between each, the total storage used may be only slightly more than 5 GB, depending on how much data changes between each backup[6](https://serverfault.com/questions/227929/approximately-how-much-would-it-cost-to-make-an-ami-image-from-a-ec2-micro-serve).

## Key Points

- **You are not charged for the full size of each AMI, but only for the unique storage they consume.**
    
- **Storage costs are based on the actual data stored in the EBS snapshots, not the number of AMIs.**
    
- **To see exact storage usage, check the EBS Snapshots section in the AWS Console.**

> “AMI sizes are not easy to calculate. This is because they are based on Amazon EBS snapshots, and EBS snapshots are incremental in nature... you will only pay for storage that does not already exist.”[3](https://stackoverflow.com/questions/55989633/aws-ec2-get-the-ami-size)

## Summary Table

| Number of AMIs | Storage Consumed      | How Charged                         |
| -------------- | --------------------- | ----------------------------------- |
| 1              | Size of used EBS data | $0.05/GB/month for snapshot         |
| 20             | Used data + changes   | Only unique blocks in each snapshot |

**In short:** With 20 AMIs, you are charged only for the unique data stored across all snapshots, not 20x the size of your instance. The cost is determined by the total EBS snapshot storage used, which is typically much less than the sum of all AMI sizes if there is significant overlap or minimal changes between backups[3](https://stackoverflow.com/questions/55989633/aws-ec2-get-the-ami-size)[6](https://serverfault.com/questions/227929/approximately-how-much-would-it-cost-to-make-an-ami-image-from-a-ec2-micro-serve).

1. [https://allcloud.io/blog/ec2-ephemeral-storage-size-per-instance-type/](https://allcloud.io/blog/ec2-ephemeral-storage-size-per-instance-type/)
2. [https://aws.amazon.com/ec2/instance-types/](https://aws.amazon.com/ec2/instance-types/)
3. [https://stackoverflow.com/questions/55989633/aws-ec2-get-the-ami-size](https://stackoverflow.com/questions/55989633/aws-ec2-get-the-ami-size)
4. [https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/AMIs.html](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/AMIs.html)
5. [https://docs.aws.amazon.com/pricing-calculator/latest/userguide/ec2-estimates.html](https://docs.aws.amazon.com/pricing-calculator/latest/userguide/ec2-estimates.html)
6. [https://serverfault.com/questions/227929/approximately-how-much-would-it-cost-to-make-an-ami-image-from-a-ec2-micro-serve](https://serverfault.com/questions/227929/approximately-how-much-would-it-cost-to-make-an-ami-image-from-a-ec2-micro-serve)
7. [https://docs.aws.amazon.com/whitepapers/latest/cost-optimization-right-sizing/tips-for-right-sizing-your-workloads.html](https://docs.aws.amazon.com/whitepapers/latest/cost-optimization-right-sizing/tips-for-right-sizing-your-workloads.html)
8. [https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/volume_limits.html](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/volume_limits.html)