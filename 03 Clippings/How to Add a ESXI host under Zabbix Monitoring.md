
# Standard Operating Procedure (SOP) for Adding an ESXi Host to Zabbix Monitoring

To effectively monitor an ESXi host using Zabbix, follow this detailed step-by-step guide. This procedure assumes that you have a working Zabbix server and access to the ESXi host.

## Prerequisites

- Ensure Zabbix Server is installed and running.
- Have administrative access to the ESXi host.
- Ensure you have the necessary credentials for the ESXi host.

## Step 1: Configure Zabbix Server for VMware Monitoring

1. **Edit Zabbix Configuration**:
- Open the Zabbix server configuration file:
	`sudo nano /etc/zabbix/zabbix_server.conf`

-  Add or modify the following lines to enable VMware monitoring:
```
StartVMwareCollectors=5
VMwareFrequency=60
VMwarePerfFrequency=60
VMwareCacheSize=32M
VMwareTimeout=120
```
- Save and exit the editor.

2. **Restart Zabbix Server**:
-  Restart the Zabbix server to apply changes:

## Step 2: Create a User on ESXi (Optional)

1. **Access ESXi**:
	- Log in to your ESXi host via SSH or use the web interface.
2. **Create a User**:
	- Create a user specifically for Zabbix monitoring if needed. This user should have sufficient permissions to access performance data.

## Step 3: Obtain the UUID of the ESXi Host

1. **Using SSH**:
	- Connect to your ESXi host and run:
	- Note down the UUID value.
	
2. **Using Web Interface**:
	- Access the Managed Object Browser (MOB) at `https://<your_esxi_host>/mob/?moid=ha-host&doPath=hardware.systemInfo` to find the UUID.

## Step 4: Import Zabbix Template for ESXi Monitoring

1. **Download Template**:
	- Obtain the VMware ESXi template XML file (e.g., `TEMPLATE – VMWARE – STANDALONE ESXI HOST.xml`).
2. **Import Template into Zabbix**:
	- Log in to the Zabbix web interface.
	- Navigate to **Configuration** -> **Templates** -> **Import**.
	- Upload the downloaded template file.

## Step 5: Add ESXi Host in Zabbix

1. **Log into Zabbix Web Interface**.
2. Navigate to **Configuration** -> **Hosts**.
3. Click on **Create host**.
4. Fill in the following details:
	- **Hostname**: Enter the UUID of your ESXi host.
	- **Visible name**: Provide a friendly name for easy identification.
	- **Groups**: Select or create a group (e.g., "VMware Hosts").
	- **Agent interfaces**: Enter the IP address of your ESXi host.
5. Under **Templates**, select the imported VMware template (e.g., "Template VM VMware").
6. In the **Macros** tab, fill in:
	- `{$URL}`: `https://<your_esxi_host>/sdk`
	- `{$USERNAME}`: Your ESXi username (e.g., `root`).
	- `{$PASSWORD}`: Your ESXi password.
7. Click on **Add** to save the configuration.

## Step 6: Verify Monitoring Data

1. Navigate to **Monitoring** -> **Latest data** in the Zabbix web interface.
2. After approximately 10-15 minutes, check if metrics from your ESXi host are appearing.

## Troubleshooting Tips
- If metrics do not appear, verify that:
	- The Zabbix server can reach the ESXi host over network ports (default port 443 for HTTPS).
	- The credentials provided in macros are correct.
	- The configuration settings in `/etc/zabbix/zabbix_server.conf` are correctly set and saved.