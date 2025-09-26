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

|                                                                                                                                                             |               |
| ----------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------- |
| [vol-056b86ce2a35346c6](https://us-west-2.console.aws.amazon.com/ec2/home?region=us-west-2#Volumes:volumeId=vol-056b86ce2a35346c6;sort=desc:createTime)<br> | /dev/xvda     |
|                                                                                                                                                             |               |
|                                                                                                                                                             |               |
| flow-tsne-testing<br>                                                                                                                                       | /dev/sda1     |
|                                                                                                                                                             |               |
| cas-offinder-instance-dev                                                                                                                                   | /dev/xvda<br> |
| cas-offinder-instance-dev                                                                                                                                   | /dev/sdf      |


![[Pasted image 20250606150809.png]]



Hi Arthur,

EBS Volume Encryption:  
We have successfully encrypted the EBS volumes listed below. Additionally, we upgraded the volume for the QB_Data_Transfer_ClinicalVPC instance from gp2 to gp3, as recommended.

Please let us know if you encounter any issues.

 Note: The instances cas-offinder-instance-dev and flow-tsne-testing (Caribou-AWS-Dev) were in a stopped state. After encrypting their volumes, we have kept them stopped.

**Instance Resizing:**  
We had previously recommended migrating the following instances to Graviton-based types. However, these instances are currently running on x86_64 Intel architecture, which is incompatible with Graviton (ARM64).

Below is an updated table with **Compute Optimizer-based alternatives** that are compatible with the existing architecture.

Please review the suggested options and potential monthly savings, and let us know your approval to proceed with the optimization.

Best regards,  
[Your Name]