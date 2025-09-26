# Types of collector
| **Collector Type**               | **Description**                                                                                | **When to Use**                                                                         |
| -------------------------------- | ---------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------- |
| **Nano Collector**               | Lightweight collector for testing purposes; minimal resource usage.                            | Use for testing environments or small-scale setups where limited monitoring is needed.  |
| **Small Collector**              | Consumes ~2GB of memory; monitors up to 200 Linux or 100 Windows resources.                    | Suitable for small environments or specific segments of larger infrastructures.         |
| **Medium Collector**             | Consumes ~4GB of memory; monitors up to 1000 Linux or 500 Windows resources.                   | Ideal for medium-sized environments with moderate monitoring needs.                     |
| **Large Collector**              | Consumes ~8GB of memory; monitors up to 2000 Linux or 750 Windows resources.                   | Best for larger environments requiring extensive monitoring capabilities.               |
| **Extra Large Collector**        | Consumes ~16GB of memory; designed for high-demand monitoring scenarios.                       | Use in environments with numerous devices and high performance requirements.            |
| **Double Extra Large Collector** | Consumes ~32GB of memory; supports very large infrastructures with extensive monitoring needs. | Necessary for enterprise-level deployments with a vast number of resources and metrics. |

# **Steps to Enable Remote Administration via Windows Firewall GUI**

1. **Open Windows Firewall with Advanced Security:**
    
    - Press `Win + R`, type `wf.msc`, and press `Enter`.
2. **Locate Inbound Rules:**
    
    - In the left pane, click **Inbound Rules**.
3. **Create a New Rule for Remote Administration:**
    
    - Click **New Rule** in the right-hand panel.
    - In the New Inbound Rule Wizard:
        - **Step 1:** Select **Predefined**.
        - **Step 2:** From the dropdown, choose **Remote Service Management**.
        - **Step 3:** Select the specific rules to enable:
            - **Remote Service Management (RPC-EPMAP)**.
            - **Remote Service Management (RPC)**.
            - **Remote Service Management (RPC-WMIC-In)**.
        - **Step 4:** Select **Allow the connection** and click **Finish**.
4. **Verify the Rule:**
    - Ensure the new rules are enabled (green checkmarks) in the **Inbound Rules** list.
5. **Test Connectivity:**
    
    - Confirm remote management is working by attempting to access the system using tools like `wmic` or `PowerShell`.


# [[Cisco Meraki Monitoring in LM]]
## Properties Needed

| meraki.api.key             |     |
| -------------------------- | --- |
| meraki.api.network         |     |
| meraki.api.network.name    |     |
| meraki.api.org             |     |
| meraki.firmware            |     |
| meraki.productType         |     |
| meraki.serial              |     |
| meraki.snmp.auth           |     |
| meraki.snmp.authToken.pass |     |
| meraki.snmp.priv           |     |
| meraki.snmp.privToken.pass |     |
| meraki.snmp.security       |     |

# LM Logs
- LM collects syslogs from the device and detects any anamolies
- The default retention period for LM logs is 30 days. The retention is based on subscription.
- The LM logs needs to be enabled from the collector config file by uncommenting and changing it to `true`
- In the search bar use the queries to check if 

# StatusFlap in Network Interfaces
##  How It Works
1. **Data Collection:**
	- The collection script retrieves two important SNMP values:
	    - **sysUpTime.0** (OID: .1.3.6.1.2.1.1.3.0), which represents how long the device has been running.
	    - **[[ifLastChange in Network interfaces || ifLastChange]]** (OID: .1.3.6.1.2.1.2.2.1.9.[ifIndex]), which indicates the uptime (in hundredths of a second) when the interface last changed its state.

2. **Calculation Logic:**
	- The script converts both values into a common unit and picks the higher uptime value (to account for possible SNMP engine resets or counter wraps).
	- For each interface instance, the script calculates the difference
		$$
		Difference (sec) = \frac{\text{sysUpTime} - \text{ifLastChange}}{100}​
		$$

	- It then compares this elapsed time against the poll interval (e.g., 120 seconds).
	- If the interface is currently up (actualStatus equals "1") **and** the elapsed time is less than the poll interval, it deduces that the interface changed state recently—that is, it “flapped.” In that case, the datapoint value is set to 1; otherwise, it is set to 0.
	- If alerting is disabled for that instance), the datapoint may be set to –1 to indicate that alerts are disabled.
## How to Verify It on the Network Device
## To confirm whether an interface is flapping, you can manually verify the SNMP values on the device:

1. **Retrieve sysUpTime:**
    - Use an SNMP GET command on OID **.1.3.6.1.2.1.1.3.0** to see the current uptime of the device.
2. **Retrieve ifLastChange for the Interface:**
    - Use an SNMP GET or WALK command on the OID **.1.3.6.1.2.1.2.2.1.9.[ifIndex]** for the specific interface (where [ifIndex] is the interface index number).
3. **Perform the Calculation:**
    - Convert the returned values (which are in Timeticks, where one tick equals 1/100th of a second) to seconds.
    - Calculate the difference: $$Difference (sec) = \frac{\text{sysUpTime} - \text{ifLastChange}}{100}​$$
    - Compare this difference to the configured poll interval (for example, 180 seconds).
    - If the difference is significantly less than the poll interval, then it indicates that the interface changed state very recently—which would trigger the statusflap datapoint to register a “flap” (value = 1).
