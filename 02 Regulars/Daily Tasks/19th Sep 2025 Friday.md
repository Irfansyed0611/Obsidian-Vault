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


Hi Jennifer,

We looked into the “Directory Unavailable” error you encountered. The Directory Service itself is active, MFA is properly configured, the VPC security rules are in place, and the RADIUS server is running fine.

This issue is often intermittent and can occur due to temporary network connectivity, DNS resolution, or authentication delays between the WorkSpaces client and the Directory Service endpoints. 

Could you please try reconnecting to the workspace again and let us know if you face the same issue.

Since the error has cleared up now, please try signing in again. If it reoccurs, we can run additional connectivity checks from the WorkSpaces client and review AWS Health Dashboard to confirm if it’s a broader service issue.



----

- Jennifer is not able to login
- Some other user sharing same registration ID as jennifer is currently connected to a workspace but when i tried his/her creds I am getting same error.
- Directory service is active
- RADIUS server is running
- Outbound rule exists from both subnets of workspace VPC
- Jennifer AD creds have not been locked or expired.

Get-ADUser -Identity "jennifer.parise" -Properties *

