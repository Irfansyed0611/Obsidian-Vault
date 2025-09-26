---
tags:
  - daily
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
Hi Tim,

We have completed the encryption for all the mentioned instance in the list you shared. Can we proceed to delete the old unencrypted volumes since the encryption of these volumes is already done. I've listed the volumes in the attached document.

Also we found an instance ao-development in Caribou AWS-Dev account whose EBS volume is not encrypted. This instance is not mentioned in the list you shared us earlier. Can you please confirm if we can encrypt the volume of this instance.
Details below :
instance_id - 
Account - 

----

So current IOPS is 6000 gp3. The IOPS was high only for two days 25th and 26th. So only during analysis the iops was high. If we assume more analysis will be done in the future using this server then we can increase the iops.
The baseline iops (3000) is free. So if we increase the baseline by 2000 then we'll be paying for 5000 ios.
For us-east-1 -> $0.005 per IOPS month. So the overall cost increase could be around

r5a.2xlarge  = 0.452
r5a.4xlarge = 0.904
r6g.4xlarge = 0.806

---

**Subject:** Approval Request for Instance Upgrade Due to High Memory Usage

Hi Behzad,

We’ve analyzed the recent performance issues and found that the instance is currently **under-provisioned** for the workload it’s handling. This under-provisioning led to **high memory utilization**, which caused **system status check failures** during your recent analysis.

Based on **AWS Compute Optimizer recommendations**, we suggest upgrading the instance to **`r5a.4xlarge`**. Below is a comparison of the current and proposed instance:

| Resource        | Current | Recommended Upgrade   |
| --------------- | ------- | --------------------- |
| vCPUs           | 8       | 16                    |
| Memory          | 64 GB   | 128 GB                |
| Additional Cost | —       | **~$0.46/hour extra** |

Could you please confirm if we have your approval to proceed with this upgrade?







