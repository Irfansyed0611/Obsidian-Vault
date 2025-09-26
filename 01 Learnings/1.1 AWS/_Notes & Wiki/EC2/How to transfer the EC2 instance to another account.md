To move an EC2 instance from one AWS account to another, you can't directly transfer the instance itself. Instead, you need to create an Amazon Machine Image (AMI) from the instance and then share that AMI with the target account. Here are the steps to do this:

1. **Create an AMI**:
    - In the source account, create a custom AMI of the instance you want to move. Ensure all required EBS data volumes are included in the AMI
	
2. **Share the AMI**:
    - Use the EC2 console or AWS CLI to share the AMI with the target account.
	
3. **Launch a New Instance**:
    - In the target account, find the shared AMI and launch a new instance from it.
	
4. **Transfer Elastic IP (if needed)**:
    - If your instance has an associated Elastic IP address, you can transfer it to the target account and associate it with the new instance.


References
[1] [Transfer EC2 instance or AMI to another account | AWS re:Post](https://repost.aws/knowledge-center/account-transfer-ec2-instance)
[2] [Migrate Resources Between AWS Accounts | AWS Architecture Blog](https://aws.amazon.com/blogs/architecture/migrate-resources-between-aws-accounts/)