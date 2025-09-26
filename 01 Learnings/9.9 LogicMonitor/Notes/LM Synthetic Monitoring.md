# To implement synthetic transaction monitoring in LogicMonitor, follow these steps:

1. [[LM Synthetic Monitoring#^157727| Install Selenium grid]]
    - Install and configure Selenium GRID, which acts as a proxy server to execute tests on designated devices. This setup is essential for running synthetic checks.
    
2. **[[LM Synthetic Monitoring#^4be3ca| Update the collector config]]**:
    - Ensure that the LogicMonitor Collector can access the Selenium GRID by updating its configuration:
        - Navigate to **Settings > Collectors** in the LogicMonitor portal.
        - Select the desired Collector and access its configuration settings.
        - Modify the `agent.conf` file to include the Selenium GRID URL in the `synthetics.selenium.environment.grid.url` property.
        - Save the changes and restart the Collector to apply the new settings. 

3. **Create Synthetic Transaction Scripts**:
    - Utilize the Selenium IDE Chrome Extension to record user interactions that simulate typical user workflows on your website or application. This tool allows you to create scripts without extensive coding knowledge. 

4. **Integrate Scripts into LogicMonitor**:
    - Upload the recorded Selenium scripts (in `.side` format) to LogicMonitor:
        - Navigate to the **Resources** section in the LogicMonitor portal.
        - Select the **Synthetics** resource.
        - Use the **Manage Resource Options** to upload your `.side` file. 

5. **Configure Synthetic Web Checks**:
    - Define the parameters for your synthetic checks:
        - Specify the frequency of the checks.
        - Choose the locations from which the checks will be executed.
        - Set alerting thresholds based on performance metrics such as response time and availability. 

6. **Monitor and Analyze Results**:
    - Access the results of your synthetic transactions in the LogicMonitor dashboard:
        - Review metrics on availability, latency, and potential issues.
        - Set up alerts to notify your team of any detected anomalies, enabling proactive issue resolution. 

By following these steps, you can effectively implement synthetic transaction monitoring in LogicMonitor, ensuring that critical user pathways are continuously tested and optimized for performance.

For a visual walkthrough, you might find this demonstration helpful:

https://youtu.be/-q_Ox0qwcZA 

----
# Installing Selenium and config change

^157727

Here’s a **detailed step-by-step guide** on installing **Selenium Grid** and updating the **LogicMonitor Collector configuration** to enable transaction synthetic monitoring.

## **Step 1: Install and Set Up Selenium Grid**

Selenium Grid allows you to run synthetic transaction monitoring tests across multiple browsers and environments.

### 1.1 Prerequisites

Ensure you have:
- **Java 11 or later** installed
```bash
 java -version
```    

If not installed, download and install it from [Oracle](https://www.oracle.com/java/technologies/javase-jdk11-downloads.html) or install it via:
```bash
sudo apt install openjdk-11-jre  # Ubuntu/Debian  
brew install openjdk@11  # macOS  
```

- **Chrome and ChromeDriver** (if running tests in Chrome)
```bash
google-chrome --version
chromedriver --version
```

### 1.2 Download Selenium Server

1. Navigate to the official Selenium website:  
    [https://www.selenium.dev/downloads/](https://www.selenium.dev/downloads/)
2. Download the latest **Selenium Server (Grid)** `.jar` file:
```bash
wget https://github.com/SeleniumHQ/selenium/releases/download/selenium-<version>/selenium-server-<version>.jar
```
    (Replace `<version>` with the latest version, e.g., `4.10.0`.)

### 1.3 Start Selenium Grid

You can run Selenium Grid in **Standalone** mode (single machine) or **Hub-Node** mode (multiple machines).
#### Option 1: Standalone Mode (For Local Testing

Start the Selenium Grid as a standalone service:
```bash
java -jar selenium-server-4.10.0.jar standalone
```
- The default port is **4444**.
- Access the Grid UI in a browser:
```
http://localhost:4444
```
#### Option 2: Hub-Node Mode (For Distributed Testing

**1. Start the Hub**  
On the **main control machine**, run:
```bash
java -jar selenium-server-4.10.0.jar hub
```

It will start the hub at:
```
http://localhost:4444
```

**2. Register Nodes**  
On other machines (nodes), run:
```bash
java -jar selenium-server-4.10.0.jar node --hub http://<hub-ip>:4444
```
Replace `<hub-ip>` with the actual IP of the hub machine.

### **1.4 Verify Grid Status**

To check if the Grid is running correctly, go to:
```
http://localhost:4444/status
```
It should return a JSON response showing the registered nodes.


---
## **Step 2: Update the LogicMonitor Collector Configuration**

^4be3ca

Once the **Selenium Grid** is running, update the **LogicMonitor Collector** to use it.

### **2.1 Identify the Collector Machine**

1. Log in to the **LogicMonitor** web portal.
2. Navigate to **Settings > Collectors**.
3. Select the desired **Collector**.
4. Note the **Collector installation directory**.

### **2.2 Edit the Collector Configuration**

1. SSH into the **Collector machine**.
    
2. Navigate to the Collector's config directory:
```bash
cd /usr/local/logicmonitor/agent/conf/
```
    (Path may vary, check your installation directory)
    
3. Open the **`agent.conf`** file using a text editor:
```bash
nano agent.conf
```

4. Locate or add the **Selenium Grid configuration**:
```properties
synthetics.selenium.environment.grid.url=http://localhost:4444
```
	(Change `localhost` to your **Selenium Grid Hub** IP if running in a distributed setup).
	
5. Save and exit (`CTRL+X`, then `Y`, then `Enter`).

### **2.3 Restart the LogicMonitor Collector**

Apply the new configuration by restarting the collector:
```bash
systemctl restart logicmonitor-agent  # Linux
```

or

```powershell
Restart-Service logicmonitor-agent  # Windows PowerShell
```

---

## **Step 3: Verify Integration**

1. Log in to **LogicMonitor**.
2. Navigate to **Settings > Resources > Synthetic Monitoring**.
3. Try running a **synthetic transaction test** (e.g., navigating to a login page).
4. If the test succeeds, Selenium Grid is properly configured.
5. If errors occur, check:
    - **Logs in `/usr/local/logicmonitor/agent/logs/agent.log`**.
    - Selenium Grid console (`http://localhost:4444/ui`).

---

## **Next Steps**

- **Upload Selenium test scripts** (`.side` files) to LogicMonitor.
- **Schedule synthetic checks** and set **alerts** for failures.
- Monitor **transaction performance** through LogicMonitor dashboards.

This setup ensures **synthetic transaction monitoring** works efficiently using **Selenium Grid and LogicMonitor Collector**! 🚀


