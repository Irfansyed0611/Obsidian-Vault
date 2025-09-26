---
tags:
  - daily
  - AWS
  - subnets
  - clippings
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

### AWS CLI command to get Subnet list

1. Command to list all the subnets in an region based in table and json format.
```sh
aws ec2 describe-subnets   --query 'Subnets[].{ID:SubnetId, Name:Tags[?Key==`Name`]|[0].Value, CIDR:CidrBlock, VPC:VpcId, AZ:AvailabilityZone, Public:MapPublicIpOnLaunch}'   --output json
```
Note: Get the output in json and use online tool to convert it to excel or csv.

2. 



