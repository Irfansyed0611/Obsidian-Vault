---
tags:
  - KT
  - nginx
  - rapid7
  - server-logs
  - sg
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

### Troubleshooting https can'tConnect alert
- login to master account from okta
- Go to bioinfoProd account. All the resources are moved to oregon from us-east-1
- check dependencies for ngnix
```
systemctl list-dependencies nginx
```
- data.mount will mount the 
- `blkid` to get the UUID
- updated the fstab with correct UUID
```
systemctl daemon-reload.
```
- restart the nginx services using systemctl restart ngnix
- If you see some additional space in the server and not under storage then it was created using some custom AMI or some volume was detached. It will take some time for it to disappear.
----
### Not able to install r7 agent due to ARM64 architecture
- go to ansible server in vir-services account
- after login go to /etc/ansible
- check hosts file it will show all the servers in vir
- in playbooks you'll find scripts
- rapid7 token is for each customer
- you can run the yml file in the ansible server or login to that server and push the patches.
- 86_x64
- ARM64

----
### What to check if we receive SG not accessible alert from clodu
Anything related to VBZ will be in services account frankfruit or experimenta frankfruit
SG unavailability reasons/alerts from cloudwatch
- while patching
- software updates done from AWS end
- If storage gateway goes down entirely.







