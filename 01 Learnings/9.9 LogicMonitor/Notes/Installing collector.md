
### **Prerequisites**
- Ensure the Windows server meets the minimum requirements:
- At least 2 GB of RAM
- Reliable time synchronization (NTP or Windows Time Services)
- English-language OS
- Comprehensive port access (outgoing HTTPS on port 443 and necessary ports for monitoring protocols)

### Installation Steps

1. Log in to LogicMonitor:
	- Navigate to Settings > Collectors in your LogicMonitor portal.
	- Click on Add > Collector to start the process.

2. Select Collector Type:
	- Choose the appropriate collector version and size based on your monitoring needs. There are two installer packages available:
	- Bootstrap: A smaller installation package (~500kB) for faster installation.
	- Full package: A larger installation package (~200MB).  
    

3. Download the Installer:
	- You can download the installer directly to your Windows server or use a command to download it via PowerShell.

4. Run the Installer:
	- After downloading, open the installer file (LogicMonitorSetup.exe) to start the Install Shield Wizard.
	- Follow the prompts in the wizard to proceed with the installation.

5. Configure Service Account:
	- During installation, you will be prompted to enter credentials for the account that the collector will run under. You can choose:
	- Local System (if not monitoring other Windows systems)
	- A domain account with local administrator permissions (if monitoring other Windows systems in the same domain)
	- Ensure that the account has the “==Log on as a service==” permission.  
	
6. Antivirus Configuration:
	- If antivirus software is running on the server, add a recursive exclusion for the LogicMonitor Collector application directory to prevent interference.

7. Complete Installation:
	- Once the installation is complete, the collector will start automatically. You can verify that the collector service is running by checking the Windows Services list (services.msc).

8. Verify Connection:
	- Return to the LogicMonitor portal and complete the Add a Collector dialog to verify that the collector can communicate with LogicMonitor's datacenters.

9. Monitor the Collector:
	- Enable monitoring for the device on which the collector is installed to track its performance metrics (CPU, memory, disk usage).