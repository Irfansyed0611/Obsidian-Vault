---
tags:
  - KT
  - S3
---
--------
## Task to complete

- [ ] 
- [ ]   

-----
##  Action items from daily meeting

| Task | Further Action item |
| ---- | ------------------- |
|      |                     |


----
## Notes

^9d3a50
### KT on S3
#### Overview
- Stratogent uses S3 buckets to store backups like Veaam backups
	- We create a bucket to store the backups. Create a user with put and get objects permissions to that bucket. Create access and secret key for this user and share it with storage team.
	- Veaam backup buckets should not be under lifecycle. They should always be in the standard because Veaam only recognizes standard so that it makes it easy for retrieval. 
- Tags for cost analysis and segregate the customer environment. - **Need more explanation**
	- If any tag value is IT then its usually Stratogent bucket
- Encryption - Most of the customers use default S3-SSE encryption. But in Jazz they use KMS-CMKs
- Access logging is used to log request to the bucket. [[Server logs vs CloudTrail logs | Reference]]
- Lifecycle policy and Intelligent tier
	- Difference between bucket policy and IAM policy
	- what will happen if I use lifecycle policy along with intelligent tier

#### S3 Inventory
- It is used to list all the objects within the bucket.
- It creates a report and stores it in destination bucket.
- The destination bucket should allow putObjects
- Can we add the report in the source bucket itself?

#### Batch Jobs
- copy jobs -> Used to move objects from IA tier to standard
- Restore jobs -> To move Deep archive or glacier tier objects to standard.
- Most of the copy jobs fail when the object size is more than 5GB
- How to do the copy or restore job if the object size is more than 5GB -> Use bash or python code
- Veaam backup buckets should not be under lifecycle. They should always be in the standard because Veaam only recognizes standard so that it makes it easy for retrieval. 

#### Storage Gateway:
move data from On-Prem to AWS. Through this the data can be added to S3 from on-prem using file shares.
NFS for Linux
SMB for windows

Explore storage lens in Amazon S3






