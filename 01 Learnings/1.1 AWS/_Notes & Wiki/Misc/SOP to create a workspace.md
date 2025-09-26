---
tags:
  - AWS
  - workspaces
  - SOP
---
**Purpose**: To establish a secure, standardized, and user-ready virtual workspace using AWS WorkSpaces, integrating Active Directory (AD), custom configurations, and automation.  

---

### **Prerequisites**  

1. **AWS Account**: With administrative privileges for IAM, WorkSpaces, Directory Service, and EC2.  
2. **Network Setup**: A VPC with public/private subnets and DNS resolution enabled.  
3. **Active Directory (AD)**: On-premises or cloud-based AD with domain admin credentials.  
4. **Software Licenses**: Required applications (e.g., Office 365, antivirus) for the custom image.  

---

### **1. Directory Setup Using AD Connector**  

**Objective**: Integrate AWS WorkSpaces with an existing Active Directory for centralized user management.  

#### **Steps**:  

1. **Navigate to AWS Directory Service**:  
	- Open the AWS Management Console.  
	- Go to **Directory Service > Directories > Set up directory**.  
   
2.  **Select AD Connector**:  
	- Choose **AD Connector** (not "Managed Microsoft AD").  
	- Select **Next**.  
	- ![[Pasted image 20250415144456.png]]

3. Enter AD Connector information
	- Select the Directory size (Small or Large)
	- Select Next
	- ![[Pasted image 20250415144811.png]]
4. **Choose VPC and subnets**: 
	- Select the **VPC** and **two public subnets** for high availability.  
	- ![[Pasted image 20250415144951.png]]
5. **Connect to AD**
	- Enter the **DNS domain name** of your existing AD (e.g., `[corp.example.com](http://corp.example.com "http://corp.example.com/")`). 
	- Specify **DNS IP addresses** of your AD domain controllers.
	- Enter the service account username and password
	- Select **Next**
	- ![[Pasted image 20250415150131.png]]
6. **Review and Create**:  
	- Verify settings and click **Create Directory**.  Wait for the directory status to change to **Active** (15–30 minutes).
   
---
### **2. Create a Custom Image**  

**Objective**: Build a golden image with pre-installed software and configurations.  
#### **Steps**:  

1. **Onboarding**:  
	- In the AWS Workspaces console, go to **WorkSpaces > Personal > Create a WorkSpace**.  
	- Select **persistent** WorkSpace and the required OS.
	- ![[Pasted image 20250415150725.png]]
2. Configure WorkSpace
	- Personal WorkSpace type
		- Select the Standard bundle
		- Select Auto stop as Running mode
		- Select Next
		- ![[Pasted image 20250415195511.png]]
	- Pool WorkSpace type
		- Enter pool name and description
		- Select a **Standard bundle**
		- Enter the **maximum session duration** in minutes. The default value is 960 minutes. After this duration, the instance is replaced with a new one. Users will be prompted to save their documents five minutes before the session ends.
		- Enter the **Disconnect timeout in minutes**. The default value is 15 minutes.
		- Enter the **Idle disconnect timeout in minutes**. The recommended value is 20 minutes.
		- ![[Pasted image 20250415202518.png]]
		- Enable application settings persistence.
		- Enter the minimum and maximum capacity to define the number users who can stream the WorkSpace concurrently.
		- ![[Pasted image 20250415204338.png]]

3. **Select the directories**
4. **Customize the WorkSpace**:  
   - Connect to the WorkSpace using the WorkSpaces client.  
   - Install required software (e.g., Office 365, VPN clients).  
   - Configure OS settings (e.g., group policies, default printers).  
   - Run Windows Update and reboot.  
1. **Create the Image**:  
   - In the WorkSpaces console, select the WorkSpace, then **Actions > Create Image**.  
   - Name the image (e.g., `Win10-Corporate-Image-2023`).  
   - Set **Image Permissions** to restrict access if needed.  
   - Wait for the image status to change to **Available** (1–2 hours).  

---

### **3. Create a Custom Bundle**  

**Objective**: Package the custom image with hardware specifications for provisioning.  

#### **Steps**:  

1. **Navigate to WorkSpaces Bundles**:  
   - Go to **WorkSpaces > Bundles > Create Bundle**.  

2. **Configure Bundle Settings**:  
   - **Image**: Select the custom image created in Step 2.  
   - **Hardware**:  
     - Choose **Performance** `PowerPro`, Standard, value, power).  
   - **Software**:  
     - Specify the OS type (e.g., Windows 10).  
   - **Description**: Add details (e.g., “Corporate Standard Bundle with Office 365”).  

3. **Create Bundle**:  
   - Click **Create Bundle** and wait for it to become **Available**.  

---

### **4. Provision a Complete Workspace**  

**Objective**: Deploy a WorkSpace using the AD Connector directory and custom bundle.  

#### **Steps**:  

1. **Launch WorkSpace**:  
   - Go to **WorkSpaces > WorkSpaces > Launch WorkSpace**.  



2. **Select Bundle**:  
   - Choose the **Custom Bundle** created in Step 3.  
   - Set **Initialization** to **Always Running** for persistent storage or set it to **Disconnect after 1 hour**.  
   - 
**Select Directory**:  
   - Choose the **AD Connector directory** created in Step 1.  

4. **Assign User**:  
   - Enter the **AD username** (e.g., `jane.doe@corp.example.com`).  
   - Check the Root volume and data volume encryption checkboxes and choose the KMS key.

5. **Tags (Optional)**:  
   - Add tags for cost tracking (e.g., `Department: Finance`).  

6. **Launch and Notify**:  
   - Click **Launch WorkSpaces**.  
   - The user receives an email with connection instructions.  

---

### **Post-Provisioning Steps**  

1. **User Access Validation**:  
   - Have the user log in and confirm access to apps/network resources.  

---