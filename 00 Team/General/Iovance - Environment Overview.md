---
tags:
  - iovance
  - AWS
  - env
---
---
## AWS Organizations
### AWS Accounts
- Iovance-Master -> This is the main account where billing is setup.
- Under core OU there are two accounts **log Archive** and **security Audit**. Both these accounts were created using **[control tower]**.
- In production OU
	- Datasystems production and Iovance-Datasystems - These accounts are taken care by Sasi Naidu.
	- Iovance AWS Workspace will be renamed as Sandbox account.
	- Manufacturing accounts are names with MFG
	- Iovance-sharedServices account has more work.
### AWS Services


## EC2
- All the EC2 instances in Iovance are in N. Virginia (us-east-1).
- Most EC2 instances have a set of security groups that allow them to communicate with applications like rapid7, monitoring-tools, security tools. One such security group is **INFRA-MGMT-VPC-Infrasructure**.
	- *How to determine which security group to add?*
- In Iovance there are multiple golden AMIs configured for specific OS. Based on the request we have to launch an instance from AMI.
- 
	![[Pasted image 20250421212157.png]]
- *Need clarification on how data lifecycle policy works in Iovance ec2.*