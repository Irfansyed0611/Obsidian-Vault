
#### Step 1: Prerequisites
Before starting the integration, ensure you have:
- Admin-level permissions in LogicMonitor for integrations.
- An active Slack account with permissions to install apps.
- Access to the Slack API portal to create a Slack app.
#### Step 2: Create a Slack App
1. Go to the Slack API portal
2. Select Create a New Slack App.
3. Enter the App Name and choose a Development Workspace.
4. Navigate to the Basic Information page and collect the following credentials:
	- App ID
	- Client ID
	- Client Secret
5. Under OAuth & Permissions, add a new redirect URL: 
	 https://company.logicmonitor.com/santaba/uiv4/exchange/cloud/slack
6. Save changes
#### Step 3: Install and Configure the LogicMonitor App for Slack

1. In LogicMonitor, navigate to Settings > Integrations.
2. Click on Add and select Slack App.
3. Enter a unique name and description for your integration.
4. Choose between:
	- Add Integration to New Workspace (if not already installed).
	- Add Integration to Existing Workspace (for additional channels).
5. If prompted, click Allow to grant access to your Slack workspace.
6. Open the LogicMonitor app in Slack (found in the left-hand menu) and click on the Get Started button.
7. In the Finish Your Install dialog:
	- Enter your LogicMonitor portal name (from your LogicMonitor URL).
	- Verify alert statuses you want routed to Slack (new alerts are mandatory).
	- Select the desired Slack channel for notifications.
8. Click on Submit to complete the integration setup.
#### Step 4: Configure Alert Routing Conditions
1. After submitting, you will see an option to configure alert routing conditions.
2. Determine which alerts should be routed based on:
	- Alert rules
	- Escalation chains
	- Recipient groups
3. You can exit this configuration process and return later if needed.

#### Step 5: Approve the App in Slack Workspaces
1. From your Enterprise Slack page (enterprise.slack.com), go to Manage Organization.
2. Select Integrations > Installed apps, find the LogicMonitor application, and click on it.
3. Choose Manage > Approve this app for each workspace you want to integrate.

#### Step 6: Utilize Bidirectional FeaturesOnce integrated, you can:
- Receive alert notifications in Slack with summary information.
- Acknowledge alerts directly from Slack.
- Schedule downtime (SDT) for resources triggering alerts from within Slack.
- Open alerts in LogicMonitor from Slack.

Use slash commands in any channel (even if LogicMonitor is not invited) for quick interactions:
- /help - Displays available commands.
- /acknowledge - Acknowledges an alert.
- /sdt - Puts a resource into scheduled downtime.
- /configure - Adjusts settings for alerts.