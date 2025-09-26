---
tags:
  - linux
  - patching
---
# Patching Commands for Debian and RHEL Distributions

## Debian-Based Distributions (Debian, Ubuntu, Mint)

### Basic Update Commands
```bash
# Update the package lists
sudo apt update

# Upgrade installed packages to their latest versions
sudo apt upgrade

# Upgrade packages and handle dependencies intelligently (may remove packages)
sudo apt dist-upgrade

# Modern equivalent of dist-upgrade
sudo apt full-upgrade
```

### Security Updates Only
```bash
# Install only security updates
sudo apt upgrade -s | grep "^Inst" | grep -i security | awk '{print $2}' | xargs sudo apt install
```

### Advanced Package Management
```bash
# Update a specific package
sudo apt install --only-upgrade <package-name>

# Automatically remove unused dependencies
sudo apt autoremove

# Clean up downloaded package files
sudo apt clean
```

### Unattended Upgrades (Automated Patching)
```bash
# Install unattended-upgrades package
sudo apt install unattended-upgrades apt-listchanges

# Configure unattended upgrades
sudo dpkg-reconfigure -plow unattended-upgrades

# Check unattended-upgrades configuration
cat /etc/apt/apt.conf.d/50unattended-upgrades
```

## RHEL-Based Distributions (RHEL, CentOS, Fedora)

### Basic Update Commands (RHEL 8/9, CentOS 8/Stream, Fedora)
```bash
# Update package lists and upgrade all packages
sudo dnf update

# Update a specific package
sudo dnf update <package-name>

# Apply only security updates
sudo dnf update --security

# Check which packages need updating
sudo dnf check-update
```

### For RHEL/CentOS 7 (using yum)
```bash
# Update all packages
sudo yum update

# Update a specific package
sudo yum update <package-name>

# Apply only security updates
sudo yum update --security

# Check available updates
sudo yum check-update
```

### RHEL-Specific Commands
```bash
# Register with Red Hat Subscription Management
sudo subscription-manager register --username=<username> --password=<password>

# Attach to a subscription
sudo subscription-manager attach --auto

# View available repositories
sudo subscription-manager repos --list

# Enable a specific repository
sudo subscription-manager repos --enable=<repository-id>
```

### Kernel Updates and Management
```bash
# Install latest kernel only (RHEL/CentOS)
sudo yum update kernel

# Install kernel-related packages
sudo dnf update kernel*

# Check installed kernels
rpm -q kernel
```

### System Upgrade (Major Version)
```bash
# For RHEL 8/9 and Fedora
sudo dnf system-upgrade download --releasever=<target_version>
sudo dnf system-upgrade reboot
```

## Additional Patching Tools

### For Debian/Ubuntu
```bash
# Install needrestart to detect services that need restarting
sudo apt install needrestart

# Check and restart services after updates
sudo needrestart
```

### For RHEL
```bash
# Install yum-utils for additional functionality
sudo dnf install yum-utils

# Check which packages need rebooting after update
sudo needs-restarting

# Check which services need restarting
sudo needs-restarting -s
```