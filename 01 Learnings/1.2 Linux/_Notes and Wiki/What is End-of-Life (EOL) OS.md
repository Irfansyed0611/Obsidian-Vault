
An **End-of-Life (EOL) OS** is an operating system version that is no longer supported by its vendor. Once an OS reaches EOL, it no longer receives:  
✅ **Security updates** (making it vulnerable to exploits)  
✅ **Bug fixes**  
✅ **Official support**  
✅ **Performance improvements**

### **How to Check EOL Status in Linux (RHEL & Debian)**

#### **📌 RHEL (Red Hat Enterprise Linux)**

1. **Check OS version**
    
    ```bash
    cat /etc/redhat-release
    ```
    
2. **Check RHEL EOL status**
    
    - Visit **Red Hat’s official support lifecycle page**:  
        👉 [https://access.redhat.com/support/policy/updates/errata](https://access.redhat.com/support/policy/updates/errata)
        
    - Find your RHEL version and check if it’s EOL.
        
3. **Check EOL programmatically (for RHEL 7 & later)**
    
    ```bash
    subscription-manager status
    ```
    
    - If **“Status: Unknown”** appears, your system might be EOL or not registered.
        

---

#### **📌 Debian**

1. **Check OS version**
    
    ```bash
    cat /etc/os-release
    ```
    
2. **Check Debian EOL status**
    
    - Visit **Debian’s lifecycle page**:  
        👉 [https://www.debian.org/releases/](https://www.debian.org/releases/)
        
    - Look for your Debian version to see if it’s still supported.
        
3. **Check EOL programmatically (for Debian-based systems)**
    
    ```bash
    curl -s https://endoflife.date/api/debian | jq '.[] | select(.cycle=="<your_version>").eol'
    ```
    
    Replace `<your_version>` with your Debian version (e.g., `10`).