# VM Performance DS filter
In order to segregate the the instances discovered in the DS. You can apply filter just before the Active discovery script in the global definition

The filter will work as a tag and should be present the device level also.

==Example: Cellares VM performance DS for corpIT, DevOps.==

# WMI Error Codes and Troubleshooting
## Error: 0x800706BA RPC Server Unavailable
### What the Error Means

- **RPC Server Unavailable**: This error indicates that your computer cannot communicate with the target server or service needed to execute the WMI command.
### Common Causes

1. **Firewall Issues**: A firewall may be blocking the necessary network ports.
2. **Service Not Running**: The required services (like Remote Procedure Call and Remote Registry) might not be active on the target machine.
3. **Network Problems**: There could be issues with network connectivity, such as incorrect DNS settings or a network outage.
4. **WMI Configuration**: WMI might not be properly configured on the target system.
### Resolution
- Execute “`netsh firewall set service RemoteAdmin enable`” from command console at the monitored host (not the host on which the Collector is running).
- **Restart Relevant Services:**
	- **RPC Services:** Restart the "Remote Procedure Call (RPC)" service.
		- net stop RpcSs
		- net start RpcSs
- RPC uses **TCP Port 135** for communication. Use PowerShell or Telnet to check connectivity:
	`Test-NetConnection -ComputerName <hostname or IP> -Port 135
- [[General Info#**Steps to Enable Remote Administration via Windows Firewall GUI** | Enabling RemoteAdmin using GUI]]

## Error: 0x80070005 – Access is denied by DCOM

**Possible Issues:** The user does not have remote access to the computer through DCOM. **Quick fix**: Give the user Remote Launch and Remote Activation permissions in dcomcnfg.

1. Click **Start**, click **Run**, type **DCOMCNFG**, and then click **OK**.
2. In the **Component Services** dialog box, expand **Component Services**, expand **Computers**, and then right-click **My Computer** and click **Properties**.
3. In the **My Computer Properties** dialog box, click the **COM Security** tab.
4. Under **Access Permissions**, click **Edit Limits**.
5. In the **Access Permission** dialog box, select the user the Collector uses in the **Group or user names** box (for example, the following figure allows the user ‘logicmonitor’ to access WMI remotely). In the **Allow** column under **Permissions for User**, select **Remote Access** and click **OK**.

![|500](https://www.logicmonitor.com/wp-content/uploads/2016/01/WMI_COMSecurity_AccessPerm_Edit.png)
1. In the **My Computer Properties** dialog box, click the **COM Security** tab.
2. Under **Launch and Activation Permissions**, click **Edit Limits**.
3. In the **Access Permission** dialog box, select the user the Collector uses in the **Group or user names** box (for example, the following figure allows the user ‘logicmonitor’ to access WMI remotely). In the **Allow** column under **Permissions for User**, select **Remote Launch** and **Remote Activation**, and then click **OK**.
![|500](https://www.logicmonitor.com/wp-content/uploads/2016/01/WMI_COMSecurity_Launchand-Activation_Edit.png)


# Collector Goes Down
The issue where your LogicMonitor collector services are running but you're receiving a "collector down" alert can stem from several potential causes. Here's a structured approach to diagnose and resolve the problem:

## 1. Network Connectivity Issues
- **Outbound Connectivity**: Ensure the collector can reach LogicMonitor's servers over HTTPS (port 443). Test using `curl` or `Invoke-WebRequest` to `https://.[logicmonitor.com](http://logicmonitor.com "http://logicmonitor.com/")`.
   - **Proxy/Firewall Rules**: If your environment uses a proxy, verify the collector is configured to use it (check `logicmonitor.conf` or proxy settings in the collector's installation directory).
   
- **DNS Resolution**: Confirm the collector can resolve LogicMonitor endpoints (e.g., `nslookup .[logicmonitor.com](http://logicmonitor.com "http://logicmonitor.com/")`).

## 2. Collector Logs Analysis
   - Check logs in the collector's `logs/` directory (e.g., `wrapper.log`, `collector.log`). Look for errors like:
     - SSL/TLS handshake failures (e.g., expired certificates).
     - Authentication issues (invalid access ID/key).
     - Network timeouts or connection refusals.

## 3. Resource Constraints
   - **High CPU/Memory/Disk Usage**: Use Task Manager or Performance Monitor to check for resource exhaustion. A resource-starved collector might run but fail to communicate.
   - **Hung Collector Process**: Restart the collector service manually to rule out a hung state.

## 4. SSL/TLS or Time Sync Issues
   - **Certificate Trust**: Ensure the Windows server trusts LogicMonitor's SSL certificates (e.g., no expired/missing certificates in the OS trust store).
   - **Time Synchronization**: Verify the server’s clock is synced with NTP. Time skews >5 minutes can break SSL.

## 5. Collector Configuration
   - **Credentials**: Validate the `access_id` and `access_key` in `agent.conf` (located in the collector’s `conf/` directory).
- **Collector Version**: Update to the [latest collector version]([https://www.logicmonitor.com/support/collectors/collector-upgrades/](https://www.logicmonitor.com/support/collectors/collector-upgrades/ "https://www.logicmonitor.com/support/collectors/collector-upgrades/")) if outdated.

## 6. Local Firewall/AV Software
   - Temporarily disable Windows Firewall or antivirus software to test if they’re blocking traffic.

## 7. Alert Configuration
   - Check if the alert is based on a **ping test** (ICMP) rather than service status. If ICMP is blocked, the alert may falsely trigger.
   - Review the LogicMonitor alert thresholds and criteria (e.g., ping timeout settings).

## Steps to Resolve:
1. Check if DNS resolution is happening fine
```powershell
	nslookup <customer>.logicmonitor.com
```
1. **Restart the Collector**:
```powershell
Restart-Service -Name "LogicMonitor Collector" -Force
```
1. **Test Connectivity**:
```powershell
Test-NetConnection -ComputerName ".[logicmonitor.com](http://logicmonitor.com "http://logicmonitor.com/")" -Port 443
```
