- Basecamp 
	- NAT Gateways - Can be deleted
	- S3 multi-part upload lifecycle - Need to check with resource owner
	- EIP - Can be deleted
	- EBS volumes - Should take snapshots and delete the volumes
	- SPF






Hi @Balaji/@Arun,

We have successfully completed the POC testing of uptime monitoring of EC2 instance. We created a custom cloudwatch metric which polls uptime of each instance every minute and we use this metric to show availability of each instance in the cloudwatch dashboard. Can we have a call to discuss the functionality of this setup?

Total cost for uptime monitoring:
- Custom uptime metric - 0.3 dollars per month
- Dashboard for availability - 3 dollars per month
