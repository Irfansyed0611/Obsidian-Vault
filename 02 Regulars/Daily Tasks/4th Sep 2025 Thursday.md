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

- Uptime in seconds as a custom metric from within the instance
	- Pros:
		- Custom metric for uptime in seconds in CW.
		- Data is accurate
		- Can be displayed in dashboard as well
	- Cons
		- Needs AWS CLI preinstalled in each instance
		- Needs to run every minute.
		- Costs 0.3$ per month per metric		
- Availability using status check metrics
	- pros
		- Easy to setup. Uses reliable `StatusCheckFailed` Metric
		- Can be displayed in dashboard
		- No additional cost 
	- Cons
		- This is availability of a server over a period of time and not the actual uptime
		- Doesn't account for the unavailability of the server when the instance is down and there is no status check failed.
- Uptime report through lambda
	- Pros
		- Easy to setup. Makes use of existing SSM setup.
		- Gets the uptime report for all instance at once in a centralized way and is stored in S3.
		- Can be configured to share a consolidated report to DLs through SNS.
	- Cons
		- Adds additional cost of lambda if the frequency of execution is increased.
		- Doesn't get uptime if the instance doesn't have active SSM agent.
		- Can't be displayed in dashboard.