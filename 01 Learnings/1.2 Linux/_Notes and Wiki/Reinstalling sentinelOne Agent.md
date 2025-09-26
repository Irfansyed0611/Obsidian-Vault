## SentinelOne Agent Removal and Reinstallation SOP

### Prerequisites

- Root/sudo access to the server
- Coordination with security team
- New SentinelOne installation package and site token
- Maintenance window scheduled

### Phase 1: Pre-Removal Assessment

**Step 1: Document Current State**

```bash
# Check current agent status
sudo /opt/sentinelone/bin/sentinelctl status

# Check agent version
sudo /opt/sentinelone/bin/sentinelctl version

# Check running processes
ps aux | grep -i sentinel

# Document installed packages
rpm -qa | grep -i sentinel

```

**Step 2: Verify Service Status**

```bash
# Check systemd service status
sudo systemctl status sentinelone

# Check if agent is communicating
sudo /opt/sentinelone/bin/sentinelctl management status

```

### Phase 2: Safe Removal

**Step 3: Stop SentinelOne Services**

```bash
# Stop the service gracefully
sudo systemctl stop sentinelone

# Verify service is stopped
sudo systemctl status sentinelone

```

**Step 4: Remove Agent Package**

```bash
# For RPM-based removal
sudo rpm -e sentinelone

# Alternative: If installed via package manager
sudo yum remove sentinelone
# or
sudo dnf remove sentinelone

```

**Step 5: Clean Residual Files**

```bash
# Remove remaining directories (be cautious)
sudo rm -rf /opt/sentinelone
sudo rm -rf /var/lib/sentinelone
sudo rm -rf /etc/sentinelone

# Remove any systemd service files
sudo rm -f /etc/systemd/system/sentinelone.service
sudo rm -f /usr/lib/systemd/system/sentinelone.service

# Reload systemd
sudo systemctl daemon-reload

```

**Step 6: Verify Complete Removal**

```bash
# Confirm no processes running
ps aux | grep -i sentinel

# Confirm no packages installed
rpm -qa | grep -i sentinel

# Check for any remaining files
find / -name "*sentinel*" -type f 2>/dev/null

```

### Phase 3: Fresh Installation

**Step 7: Prepare for Installation**

```bash
# Update system packages
sudo yum update -y

# Ensure required dependencies are available
sudo yum install -y curl wget

```

**Step 8: Install New Agent**

```bash
# Download the agent (replace with actual download link from your console)
wget [SentinelOne_Agent_Download_URL]

# Make installer executable
chmod +x SentinelOne_Agent_Package.rpm

# Install the agent
sudo rpm -i SentinelOne_Agent_Package.rpm

```

**Step 9: Configure Agent**

```bash
# Set site token (get from SentinelOne console)
sudo /opt/sentinelone/bin/sentinelctl management token set [YOUR_SITE_TOKEN]

# Set management server URL if needed
sudo /opt/sentinelone/bin/sentinelctl management url set [MANAGEMENT_SERVER_URL]

```

**Step 10: Start and Enable Service**

```bash
# Start the service
sudo systemctl start sentinelone

# Enable auto-start
sudo systemctl enable sentinelone

# Verify service is running
sudo systemctl status sentinelone

```

### Phase 4: Verification

**Step 11: Verify Agent Status**

```bash
# Check agent status
sudo /opt/sentinelone/bin/sentinelctl status

# Check management connectivity
sudo /opt/sentinelone/bin/sentinelctl management status

# Verify agent info
sudo /opt/sentinelone/bin/sentinelctl info

```

**Step 12: Console Verification**

- Log into SentinelOne management console
- Navigate to Endpoints/Agents
- Verify the server appears in the console
- Check agent status shows as "Online" or "Protected"
- Verify last seen timestamp is recent

### Phase 5: Post-Installation

**Step 13: Final Checks**

```bash
# Monitor logs for any errors
sudo tail -f /var/log/sentinelone/agent.log

# Test connectivity
sudo /opt/sentinelone/bin/sentinelctl management test-connectivity

```

**Step 14: Documentation**

- Update server documentation with new agent version
- Notify security team of successful reinstallation
- Schedule follow-up verification in 24 hours

### Troubleshooting Notes

If agent still doesn't appear in console:

- Verify site token is correct
- Check firewall rules for required ports (typically 443, 80)
- Ensure DNS resolution for management server
- Check proxy settings if applicable
- Review agent logs for specific error messages

### Important Safety Considerations

- Always coordinate with security team before removal
- Ensure you have the correct site token and installation package
- The server will be unprotected during the removal/installation gap
- Consider scheduling during maintenance windows
- Keep installation packages and tokens secure