
--------
## Task to complete

- [x] Daily standup call
- [x] BU2 Team3 SDM meeting
- [x] BU1 Team1 Meeting.

-----
##  Notes
Hi Karan and Lohith,

As discussed, below are the findings and MOM from the meeting:

**Meeting summary:**
Date: 8/01/2025
attendees: @Mohammed Yaseen @Karan Bhatta @Lohith Kumar D @Syed Irfan@Sathish S
  

Findings:
1. Interface Alerts: The alerts on ports 1/1/8, 1/1/31, LAG9, and LAG51 were valid and resulted from planned activities, as confirmed all other interface alerts are genuine and expected.

2. System Resource Alerts: The CPU utilization alerts (cpu_pct_used and cpu_wait) on firewalls were caused by the recent upgrade. As confirmed, this behaviour is expected, and the alert interval will be adjusted from 9 minutes to 30 minutes as a workaround.

3. Alert Modifications: As per Gerson's confirmation, the alert for LAG9 on the Core2 switch and port 1/1/8 on the core switch will be disabled.
 

**MOM:**

| Topic                         | Action owner     | Action                                                          |
| ----------------------------- | ---------------- | --------------------------------------------------------------- |
| Alerts on interfaces          | @Lohith Kumar D  | Ensure devices/interfaces are added to SDT before any activity. |
| System resource alerts        | @Monitoring Team | Change the alert triggering interval to 30 mins.                |
| lag9 and 1/1/8 to be disabled | @Monitoring Team | To disable alerting as per customer confirmation.               |