## Create a transaction monitor with the Add Transaction wizard

### Step 1: Preparation

#### 1.1 Prerequisites

- **SolarWinds Account**: Ensure you have access to the SolarWinds platform with appropriate permissions to configure WPM.
- **Web Application URL**: Have the URL of the web application you want to monitor.
- **Credentials**: Obtain valid user credentials for the web application to simulate login and other interactions.

#### 1.2 System Requirements

Ensure that your environment meets the following requirements:

- **Web Transaction Recorder**: Installed on a server or remote system that meets the minimum requirements (e.g., CPU: 1 GHz, Memory: 2 GB).
- **WPM Player Service**: Deployed on remote systems if necessary for accurate performance sampling.
### Step 2: Create a New Recording

#### 2.1 Start Recording

1. Click on **Record Transactions** or similar option in the WPM section.
2. The Web Transaction Recorder will launch.

#### 2.2 Define Transaction Steps

1. **Navigate to the Login Page**:
    
    - Enter the URL of your web application’s login page.
    
2. **Simulate User Actions**:
    
    - Input valid credentials for login and perform any additional steps you want to monitor (e.g., navigating to a dashboard).
    - The recorder will capture all actions, including button clicks and form submissions.
    
3. **Stop Recording**:
    
    - Once all steps are recorded, stop the recording session.
    
4. **Save Recording**:
    
    - Name your recording appropriately (e.g., "Login Transaction") and save it.

### Step 3. Add transaction
1. To create a transaction monitor, select a recording and assign it to a playback location:
	1. Click Settings > All Settings > WPM Settings.
	2. Click Add a Transaction Monitor to open the Add Transaction wizard.
	3. Select a recording and click Next.

2. To import a recording, click Import, browse to the file, and click Import.
	![[Pasted image 20241217034604.png]]
3.  Select the location where you want to play the recording, and then click Next.
	![[Pasted image 20241217035020.png]]
4. To add a new location, see [Add transaction locations in WPM](https://documentation.solarwinds.com/en/success_center/wpm/content/orionwpmagaddingalocation.htm).

5. After you assign a recording to a location, define the properties for the transaction monitor.
	1. Enter a Description.
	2. Select a Playback interval to specify how frequently you want to play the transaction.
	3. Select [Set thresholds in WPM](https://documentation.solarwinds.com/en/success_center/wpm/content/orionseumagtransactionsthresholds.htm) for each step in the transaction.
	4. (Optional) To use a proxy server, click Advanced, and enter the server address in the Proxy URL field. For details, see [Configure proxies for WPM transaction locations and transactions](https://documentation.solarwinds.com/en/success_center/wpm/content/orionseumphproxy.htm).
		![[Pasted image 20241217041257.png]]
6. To enable screen captures, click Advanced, and enable that option.
7. Click Save Monitor.
		![[Pasted image 20241217041335.png]]
