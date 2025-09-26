---
title: "Cisco Meraki Monitoring"
source: "https://www.logicmonitor.com/support/cisco-meraki-monitoring"
author:
  - "[[LogicMonitor]]"
published: 2023-09-26
created: 2024-12-18
description: "Provides how LogicMonitor modules monitors Cisco Meraki devices including Wireless Access Points and collects data"
tags:
  - "clippings"
---
**Disclaimer:** These modules monitor Cisco Meraki devices as discrete LogicMonitor Resources and are not in-place upgrades to Cisco Meraki (Legacy) modules. For more information, see [Cisco Meraki (Legacy)](https://www.logicmonitor.com/support/monitoring/networking-firewalls/cisco-meraki-monitoring-legacy).

Production use of these modules in parallel with Cisco Meraki (Legacy) modules (or other 3rd party monitoring solutions) to monitor the same Meraki Devices in the same Meraki Networks is not recommended. Doing so can cause Meraki Dashboard API 429 errors and data gaps across both modules and third-party monitoring solutions. To mitigate risk, follow your organization’s change control processes.

This package of LogicMonitor modules monitors Cisco Meraki devices including Wireless Access Points, Smart Cameras, Security Appliances, Switches, Cellular Gateways, and Environmental Sensors. These modules collect data using both the Cisco Meraki Dashboard API and snmp.meraki.com. For more information, see the [Meraki Dashboard API](https://developer.cisco.com/meraki/api/) documentation from Cisco.

## Requirements

- Collector version 32.400 or later
- An API Token and Meraki Organization ID using the Cisco Meraki portal or Cisco Meraki API. For more information, see the following Meraki documentation:
- [Create API Token](https://developer.cisco.com/meraki/api/authorization/#obtaining-your-meraki-api-key)
- [How do I find my Org ID?](https://developer.cisco.com/meraki/api-v1/getting-started/)
- Port 16100 configured to allow access to snmp.meraki.com  
For more information, see the [SNMP Overview and Configuration](https://documentation.meraki.com/General_Administration/Monitoring_and_Reporting/SNMP_Overview_and_Configuration) documentation from Cisco.

## Add Resources into Monitoring

You can add Cisco Meraki resource using the following methods:

- (Recommended) Advanced NetScan that uses the Meraki Dashboard API to discover and create Resources for each monitored Meraki Device. The NetScan organizes each Resource in Resource Groups for each Meraki Network. For more information, see [What is NetScan](https://www.logicmonitor.com/support/settings/netscan-policies/what-is-netscan).
- Manually

**Warning:** To avoid overages, verify that you have sufficient LogicMonitor licenses before running the NetScan. In addition, test your NetScan filters to ensure you do not inadvertently add Meraki devices you do not want to monitor.

### Using Advanced NetScan to add Cisco Meraki Resources (Recommended)

For more information about using Advanced NetScan, see [Enhanced Script Netscan](https://www.logicmonitor.com/support/enhanced-script-netscan).

1. In your **LogicMonitor Portal** \> **Modules** \> **Exchange**, install the Cisco Meraki modules. For a list of modules, see [LogicModules in Package](https://www.logicmonitor.com/support/cisco-meraki-monitoring#h-logicmodules-in-package).
2. Navigate to **Resources > Add > Several Devices > Advanced NetScan**.  
The **Add Advanced NetScan** page is displayed.
3. Enter a name that you want to associate with this NetScan. For example, “Cisco Meraki Devices”.
4. Select the Collector to execute the NetScan.  
For more information, see [Collector Assignment](https://www.logicmonitor.com/support/enhanced-script-netscan#h-collector-assignment) in [Enhanced Script Netscan](https://www.logicmonitor.com/support/enhanced-script-netscan).
5. Select **Enhanced Script NetScan** from the **Method** drop-down list.
6. From the Enhanced Script section, select **Device Credentials** > Use custom credentials for this scan.
7. Add the following properties that provide the NetScan with the required Cisco Meraki credentials and change how and where the NetScan creates and organizes resources:

| Property | Value | Required |
| --- | --- | --- |
| meraki.api.org | Meraki Org ID | Yes |
| meraki.api.key | Meraki API Key   The NetScan masks the value of this property. | Yes |
| meraki.api.key.addToAllDevices | Whether to set meraki.api.key on every device. Can be set manually on the group folder. Defaults to true. | No |
| meraki.snmp.community.pass   (Applies to SNMP v1 or v2c) | Community string  **Note:** Configure these SNMP settings as Netscan Credential Properties or as Custom Properties of the Resource Group that contains your Meraki devices. Obtain the values from your Meraki Dashboard -> Organization -> Settings -> SNMP. These settings only affect communication with snmp.meraki.com. If you are also using local SNMP DataSources, like SNMP\_Network\_Interfaces or SNMP\_Host\_Uptime, these settings will not alter their settings or operation. | Yes   (If using SNMP v2c) |
| meraki.snmp.security   (Applies to SNMP v3) | Username  **Note:** Configure these SNMP settings as Netscan Credential Properties or as Custom Properties of the Resource Group that contains your Meraki devices. Obtain the values from your Meraki Dashboard -> Organization -> Settings -> SNMP. These settings only affect communication with snmp.meraki.com. If you are also using local SNMP DataSources, like SNMP\_Network\_Interfaces or SNMP\_Host\_Uptime, these settings will not alter their settings or operation. | Yes   (If using SNMP v3) |
| meraki.snmp.auth   (Applies to SNMP v3) | SHA or MD5  **Note:** Configure these SNMP settings as Netscan Credential Properties or as Custom Properties of the Resource Group that contains your Meraki devices. Obtain the values from your Meraki Dashboard -> Organization -> Settings -> SNMP. These settings only affect communication with snmp.meraki.com. If you are also using local SNMP DataSources, like SNMP\_Network\_Interfaces or SNMP\_Host\_Uptime, these settings will not alter their settings or operation. | Yes   (If using SNMP v3) |
| meraki.snmp.authToken.pass   (Applies to SNMP v3) | SHA or MD5 authentication token (password)  **Note:** Configure these SNMP settings as Netscan Credential Properties or as Custom Properties of the Resource Group that contains your Meraki devices. Obtain the values from your Meraki Dashboard -> Organization -> Settings -> SNMP. These settings only affect communication with snmp.meraki.com. If you are also using local SNMP DataSources, like SNMP\_Network\_Interfaces or SNMP\_Host\_Uptime, these settings will not alter their settings or operation. | Yes   (If using SNMP v3) |
| meraki.snmp.priv   (Applies to SNMP v3) | AES128 or DES  **Note:** Configure these SNMP settings as Netscan Credential Properties or as Custom Properties of the Resource Group that contains your Meraki devices. Obtain the values from your Meraki Dashboard -> Organization -> Settings -> SNMP. These settings only affect communication with snmp.meraki.com. If you are also using local SNMP DataSources, like SNMP\_Network\_Interfaces or SNMP\_Host\_Uptime, these settings will not alter their settings or operation. | Yes   (If using SNMP v3) |
| meraki.snmp.privToken.pass   (Applies to SNMP v3) | Privacy token (password)  **Note:** Configure these SNMP settings as Netscan Credential Properties or as Custom Properties of the Resource Group that contains your Meraki devices. Obtain the values from your Meraki Dashboard -> Organization -> Settings -> SNMP. These settings only affect communication with snmp.meraki.com. If you are also using local SNMP DataSources, like SNMP\_Network\_Interfaces or SNMP\_Host\_Uptime, these settings will not alter their settings or operation. | Yes   (If using SNMP v3) |
| meraki.api.org.folder | The name of the LogicMonitor Resource Group that this NetScan creates or uses if already existing   The value can be a nested child folder. For example, folder/folder/folder. | No |
| meraki.api.org.name | Your Meraki Org Name   This becomes the name of the root Resource Group for Cisco Meraki, or becomes the name of top-level child Resource Group if you use the meraki.api.org.name property to specify an alternate root folder. | No |
| meraki.api.org.networks | Comma-separated network IDs to include (others are excluded) | No |
| meraki.api.org.collector.networks.csv | By default, new resources created by the NetScan are assigned to the collector that runs the NetScan. This property uses a CSV file to override the default behavior and assign Meraki devices to your desired collectors based on Meraki Network ID and LogicMonitor Collector names. The CSV file must be stored on the collector that executes the NetScan: Linux: /usr/local/logicmonitor/agent/bin Windows: C:\\Program Files\\LogicMonitor\\Agent\\bin For more information, see [Mapping Cisco Meraki Devices to LM Envision Collectors](https://www.logicmonitor.com/support/cisco-meraki-monitoring#h-mapping-cisco-meraki-devices-to-lm-envision-collectors). | No |
| meraki.service.url | Alternate region URLs include: https://dashboard.meraki.cn/v1 | No |
| lmaccess.id   OR   logicmonitor.access.id | A LogicMonitor API access id to search for duplicate resources in the NetScan prior to adding them to the portal. For more information on portal monitoring, see [LogicMonitor Portal Monitoring](https://www.logicmonitor.com/support/logicmonitor-portal-monitoring). | Yes |
| lmaccess.key   OR   logicmonitor.access.key | A LogicMonitor API key to search for duplicate resources in the NetScan prior to adding them to the portal. For more information on portal monitoring, see [LogicMonitor Portal Monitoring](https://www.logicmonitor.com/support/logicmonitor-portal-monitoring). | Yes |
| hostname.source | Allows for selection of the source of the hostname that is used by the NetScan. This is useful in preventing duplicate device creation when conflicts are found for existing resources. For more information, see [NetScan Troubleshooting](https://www.logicmonitor.com/support/cisco-catalyst-sd-wan-monitoring#h-netscan-troubleshooting). | No |
| skip.device.dedupe | Allows you to skip device deduplication checks, making LogicMonitor API credentials not required. | No |
| meraki.disableswitches | Disable per-switch API calls and data collection  When setting the value to “true”, configure standard SNMP properties from the URL and local device SNMP data collection using the Meraki Dashboard. In addition, disable, unapply, or uninstall the Cisco\_Meraki\_SwitchInterfaces module when verifying local SNMP data collection using SNMP\_Network\_Interfaces.  **Note:** Disabling the DataSource retains historical data, and unapplying or uninstalling the DataSource does not.  **Recommendation:** If you have thousands of switches use this setting to avoid a rate limit error or scale issues.     For more information, see the [SNMP Overview and Configuration](https://documentation.meraki.com/General_Administration/Monitoring_and_Reporting/SNMP_Overview_and_Configuration#Configuration) documentation from Cisco Meraki. | No |
| meraki.disable.network.events | Disables Meraki Network Events LogSource data collection.  **Recommendation:** Use setting if forwarding as Cisco Meraki syslog duplicates. Disable setting if you receive a rate limit. | No |
| meraki.disable.security.events | Disables Meraki Security Events LogSource data collection. | No |
| meraki.disable.assurance.alerts | Disables Assurance Alerts Events LogSource data collection. | No. |

^b199ba

8. In the Filters section, use the following filter properties to omit specific Resources (For example, appliances, switches, wireless access points, cameras, or cellular gateways) from discovery.  
These filters behave the same way as Active Discovery filters. The following device level properties are automatically discovered and can be filtered on. Available filter operations include: **Equal, NotEqual, GreaterThan, GreaterEqual, LessThan, LessEqual, Contain, NotContain, Exist, NotExist, RegexMatch, RegexNotMatch**.

**Recommendation:** To verify desired functionality, start with a specific filter of some devices you want to monitor, then incrementally change the filter operations or values to onboard monitored resources in stages. For example, you can configure the filters to only discover or import one Meraki Network at a time until you are confident that your Properties and Filters are properly defined to achieve your desired outcome.

**Warning:** If you do not configure filters, NetScan attempts to add all discovered Meraki Devices as discrete LogicMonitor Resources.

<table><tbody><tr><td>Property</td><td>Operation</td><td>value</td><td>Example</td><td>Required</td></tr><tr><td rowspan="6">meraki.productType</td><td>Equal</td><td>Import this type of Meraki devices</td><td>appliance</td><td>No</td></tr><tr><td>Equal</td><td>Import this type of Meraki devices</td><td>switch</td><td>No</td></tr><tr><td>Equal</td><td>Import this type of Meraki devices</td><td>wireless</td><td>No</td></tr><tr><td>NotEqual</td><td>Exclude this type of Meraki devices</td><td>camera</td><td>No</td></tr><tr><td>Equal</td><td>Import this type of Meraki devices</td><td>cellularGateway</td><td>No</td></tr><tr><td>Equal</td><td>Import this type of Meraki devices</td><td>sensor</td><td>No</td></tr><tr><td rowspan="3">meraki.tags</td><td>Equal</td><td>Import devices with this Meraki Dashboard tag</td><td>production</td><td>No</td></tr><tr><td>Equal</td><td>Import devices with this Meraki Dashboard tag</td><td>monitor</td><td>No</td></tr><tr><td>NotEqual</td><td>Exclude devices with this Meraki Dashboard tag</td><td>staging</td><td>No</td></tr><tr><td rowspan="3">meraki.serial</td><td>Equal</td><td>Import a device that has this Meraki serial number</td><td>Q1AB-2CD3-EFGH</td><td>No</td></tr><tr><td>Equal</td><td>Import a device that has this Meraki serial number</td><td>Q2AB-3CD4-EFGH</td><td>No</td></tr><tr><td>NotEqual</td><td>Exclude a device that has this Meraki serial number</td><td>Q3AB-4CD5-EFGH</td><td>No</td></tr><tr><td rowspan="3">meraki.firmware</td><td>Equal</td><td>Import Meraki devices having this firmware version</td><td>switch-16-4</td><td>No</td></tr><tr><td>NotEqual</td><td>Exclude Meraki devices having this firmware version</td><td>wireless-29-5-1</td><td>No</td></tr><tr><td>NotEqual</td><td>Exclude Meraki devices having this firmware version</td><td>Firmware locked. Contact support.</td><td>No</td></tr><tr><td rowspan="2">meraki.api.network.name</td><td>Equal</td><td>Import devices in this Meraki Network</td><td>Store 105</td><td>No</td></tr><tr><td>NotEqual</td><td>Exclude devices in this Meraki Network</td><td>Store 106</td><td>No</td></tr></tbody></table>

9. Select **Embed a Groovy script** and embed the following script:

**Warning:** Do not edit the script. Edited Enhanced Script NetScans are not supported. If the LogicMonitor-provided script is edited, LogicMonitor Support can require you to overwrite your edits with the supported script if problems arise. The Enhanced Script NetScan limits LM Envision Resource creation to <= 600 per hour. To create more than 600 Resources, schedule the NetScan to recur each hour until all resources are added.

```
/*******************************************************************************
*  © 2007-2024 - LogicMonitor, Inc. All rights reserved.
 ******************************************************************************/

import com.santaba.agent.groovy.utils.GroovyScriptHelper as GSH
import com.logicmonitor.mod.Snippets
import com.santaba.agent.AgentVersion
import java.text.DecimalFormat
import groovy.json.JsonOutput
import groovy.json.JsonSlurper

// To run in debug mode, set to true
Boolean debug = false
// To enable logging, set to true
Boolean log = false

// Set props object based on whether or not we are running inside a netscan or debug console
def props
try {
    hostProps.get("system.hostname")
    props = hostProps
    debug = true  // set debug to true so that we can ensure we do not print sensitive properties
}
catch (MissingPropertyException) {
    props = netscanProps
}

String org = props.get("meraki.api.org")
String key = props.get("meraki.api.key")
if (!org) {
    throw new Exception("Must provide meraki.api.org to run this script.  Verify necessary credentials have been provided in Netscan properties.")
}
if (!key) {
    throw new Exception("Must provide meraki.api.key credentials to run this script.  Verify necessary credentials have been provided in Netscan properties.")
}

def logCacheContext = "${org}::cisco-meraki-cloud"
Boolean skipDeviceDedupe = props.get("skip.device.dedupe", "false").toBoolean()
String hostnameSource    = props.get("hostname.source", "")?.toLowerCase()?.trim()

Integer collectorVersion = AgentVersion.AGENT_VERSION.toInteger()
 
// Bail out early if we don't have the correct minimum collector version to ensure netscan runs properly
if (collectorVersion < 32400) {
    def formattedVer = new DecimalFormat("00.000").format(collectorVersion / 1000)
    throw new Exception("Upgrade collector running netscan to 32.400 or higher to run full featured enhanced netscan. Currently running version ${formattedVer}.")
}

def modLoader = GSH.getInstance(GroovySystem.version).getScript("Snippets", Snippets.getLoader()).withBinding(getBinding())
def emit        = modLoader.load("lm.emit", "1.1")
def lmDebugSnip = modLoader.load("lm.debug", "1")
def lmDebug     = lmDebugSnip.debugSnippetFactory(out, debug, log, logCacheContext)
def httpSnip    = modLoader.load("proto.http", "0")
def http        = httpSnip.httpSnippetFactory(props)
def cacheSnip   = modLoader.load("lm.cache", "0")
def cache       = cacheSnip.cacheSnippetFactory(lmDebug, logCacheContext)
// Only initialize lmApi snippet class if customer has not opted out
def lmApi
if (!skipDeviceDedupe) {
    def lmApiSnippet = modLoader.load("lm.api", "0")
    lmApi = lmApiSnippet.lmApiSnippetFactory(props, http, lmDebug)
}
def ciscoMerakiCloudSnip = modLoader.load("cisco.meraki.cloud", "0")
def ciscoMerakiCloud     = ciscoMerakiCloudSnip.ciscoMerakiCloudSnippetFactory(props, lmDebug, cache, http)

String orgDisplayname      = props.get("meraki.api.org.name") ?: "MerakiOrganization"
String rootFolder          = props.get("meraki.api.org.folder") ? props.get("meraki.api.org.folder") + "/" : ""
String serviceUrl          = props.get("meraki.service.url")
def disableSwitches        = props.get("meraki.disableswitches")?.toBoolean() ?: false
def addDeviceApiKey        = props.get("meraki.api.key.addToAllDevices") == null ? true : props.get("meraki.api.key.addToAllDevices").toBoolean()
def networksWhitelist      = props.get("meraki.api.org.networks")?.tokenize(",")?.collect{ it.trim() }
def collectorNetworksCSV   = props.get("meraki.api.org.collector.networks.csv")
def collectorNetworkInfo
if (collectorNetworksCSV) collectorNetworkInfo = processCollectorNetworkInfoCSV(collectorNetworksCSV)

String merakiSnmpCommunity = props.get("meraki.snmp.community.pass") 
String merakiSnmpSecurity  = props.get("meraki.snmp.security") 
String merakiSnmpAuth      = props.get("meraki.snmp.auth") 
String merakiSnmpAuthToken = props.get("meraki.snmp.authToken.pass") 
String merakiSnmpPriv      = props.get("meraki.snmp.priv") 
String merakiSnmpPrivToken = props.get("meraki.snmp.privToken.pass") 

// Get information about devices that already exist in LM portal
List fields = ["name", "currentCollectorId", "displayName"]
Map args = ["size": 1000, "fields": fields.join(",")]
def lmDevices
// But first determine if the portal size is within a range that allows us to get all devices at once
def pathFlag, portalInfo, timeLimitSec, timeLimitMs
if (!skipDeviceDedupe) {
    portalInfo = lmApi.apiCallInfo("Devices", args)
    timeLimitSec = props.get("lmapi.timelimit.sec", "60").toInteger()
    timeLimitMs = (timeLimitSec) ? Math.min(Math.max(timeLimitSec, 30), 120) * 1000 : 60000 // Allow range 30-120 sec if configured; default to 60 sec

    if (portalInfo.timeEstimateMs > timeLimitMs) {
        lmDebug.LMDebugPrint("Estimate indicates LM API calls would take longer than time limit configured.  Proceeding with individual queries by display name for each device to add.")
        lmDebug.LMDebugPrint("\t${portalInfo}\n\tNOTE:  Time limit is set to ${timeLimitSec} seconds.  Adjust this limit by setting the property lmapi.timelimit.sec.  Max 120 seconds, min 30 seconds.")
        pathFlag = "ind"
    }
    else {
        lmDebug.LMDebugPrint("Response time indicates LM API calls will complete in a reasonable time range.  Proceeding to collect info on all devices to cross reference and prevent duplicate device creation.\n\t${portalInfo}")
        pathFlag = "all"
        lmDevices = lmApi.getPortalDevices(args)
    }
}

List<Map> resources = []

def now = new Date()
def dateFormat = "yyyy-MM-dd'T'HH:mm:ss.s z"
TimeZone tz = TimeZone.getDefault()
Map duplicateResources = [
    "date" : now.format(dateFormat, tz),
    "message" : "Duplicate device names and display names, keyed by display name that would be assigned by the netscan, found within LogicMonitor portal.  Refer to documentation for how to resolve name collisions using 'hostname.source' netscan property.",
    "total" : 0,
    "resources" : []
]

// Gather data from cache if running in debug otherwise make API requests
def orgDevicesStatuses
if (debug) {
    orgDevicesStatuses = cache.cacheGet("${org}::organizationDeviceStatuses")
    if (orgDevicesStatuses) orgDevicesStatuses = ciscoMerakiCloud.slurper.parseText(orgDevicesStatuses).values()
} else {
    orgDevicesStatuses = ciscoMerakiCloud.httpGetWithPaging("/organizations/${org}/devices/availabilities")
    if (orgDevicesStatuses) orgDevicesStatuses = ciscoMerakiCloud.slurper.parseText(orgDevicesStatuses)
}

// Organization device status data is required; we cannot proceed without it
if (!orgDevicesStatuses) {
    throw new Exception("Error occurred during organizations/${org}/devices/statuses HTTP GET: ${orgDevicesStatuses}.")
}

def orgDevices
if (debug) {
    orgDevices = cache.cacheGet("${org}::organizationDevices")
    if (orgDevices) orgDevices = ciscoMerakiCloud.slurper.parseText(orgDevices).values()
} else {
    orgDevices = ciscoMerakiCloud.httpGetWithPaging("/organizations/${org}/devices")
    if (orgDevices) orgDevices = ciscoMerakiCloud.slurper.parseText(orgDevices)
}

def orgDeviceAddresses
if (debug) {
    orgDeviceAddresses = cache.cacheGet("${org}::organizationDeviceAddresses")
    if (orgDeviceAddresses) orgDeviceAddresses = ciscoMerakiCloud.slurper.parseText(orgDeviceAddresses).values()
} else {
    orgDeviceAddresses = ciscoMerakiCloud.httpGetWithPaging("/organizations/${org}/devices/uplinks/addresses/byDevice")
    if (orgDeviceAddresses) orgDeviceAddresses = ciscoMerakiCloud.slurper.parseText(orgDeviceAddresses)
}

def orgNetworkNames = [:]
def orgNetworks
if (debug) {
    orgNetworks = cache.cacheGet("${org}::organizationNetworks")
    if (orgNetworks) orgNetworks = ciscoMerakiCloud.slurper.parseText(orgNetworks).values()
} else {
    orgNetworks = ciscoMerakiCloud.httpGetWithPaging("/organizations/${org}/networks")
    if (orgNetworks) orgNetworks = ciscoMerakiCloud.slurper.parseText(orgNetworks)
}

// Network data is required; we cannot proceed without it
if (!orgNetworks) {
    throw new Exception("Error occurred during organizations/${org}/networks HTTP GET: ${orgNetworks}.")
}

orgNetworks.each { orgNetwork ->
    def networkId = orgNetwork.get("id")
    def networkName = orgNetwork.get("name")
    orgNetworkNames.put(networkId, networkName)
}

orgDevicesStatuses.each { orgDevice ->
    def networkId   = orgDevice.get("network")?.get("id")
    def networkName = orgNetworkNames.get(networkId)
    // Verify this network should be included based on customer whitelist configuration
    if (networksWhitelist != null && !networksWhitelist.contains(networkId)) return

    def productType = orgDevice.get("productType")
    def serial = orgDevice.get("serial")
    def model = orgDevices.find { it.serial == serial}?.model
    def name = orgDevice.get("name")
    def ip = orgDeviceAddresses.find { it.serial == serial }?.uplinks?.addresses?.find { !it.isEmpty() }?.address?.first()

    // Skip devices that do not have an IP and name; we must have at least one to create a device
    if (!ip && !name) return

    String displayName = name

    // Check for existing device in LM portal with this displayName; set to false initially and update to true when dupe found
    def deviceMatch = false
    // If customer has opted out of device deduplication checks, we skip the lookups where we determine if a match exists and proceed as false
    if (!skipDeviceDedupe) {
        if (pathFlag == "ind") {
            deviceMatch = lmApi.findPortalDevice(displayName, args)
            if (!deviceMatch) deviceMatch = lmApi.findPortalDeviceByName(ip, args)
        }
        else if (pathFlag == "all") {
            deviceMatch = lmApi.checkExistingDevices(displayName, lmDevices)
            if (!deviceMatch) deviceMatch = lmApi.checkExistingDevicesByName(ip, lmDevices)
        }
    }
    if (deviceMatch) {
        // Log duplicates that would cause additional devices to be created; unless these entries are resolved, they will not be added to resources for netscan output
        def collisionInfo = [
            (displayName) : [
                "Netscan" : [
                    "hostname"    : ip,
                    "displayName" : displayName
                ],
                "LM" : [
                    "hostname"    : deviceMatch.name,
                    "collectorId" : deviceMatch.currentCollectorId,
                    "displayName" : deviceMatch.displayName
                ],
                "Resolved" : false
            ]
        ]

        // If user specified to use LM hostname on display name match, update hostname variable accordingly
        // and flag it as no longer a match since we have resolved the collision with user's input
        if (hostnameSource == "lm" || hostnameSource == "logicmonitor") {
            ip = deviceMatch.name
            deviceMatch = false
            collisionInfo[displayName]["Resolved"] = true
        }
        // If user specified to use netscan data for hostname, update the display name to make it unique
        // and flag it as no longer a match since we have resolved the collision with user's input
        else if (hostnameSource == "netscan") {
            // Update the resolved status before we change the displayName
            collisionInfo[displayName]["Resolved"] = true
            displayName = "${displayName} - ${ip}"
            deviceMatch = false
        }

        duplicateResources["resources"].add(collisionInfo)
    }

    // Verify we have minimum requirements for device creation
    if ((ip || productType == "sensor") && networkId && productType && serial) {
        if (ip == "127.0.0.1") ip = name
        if (!name) name = ip
        def deviceProps = [
            "meraki.api.org"          : emit.sanitizePropertyValue(org),
            "meraki.api.network"      : emit.sanitizePropertyValue(networkId),
            "meraki.api.network.name" : emit.sanitizePropertyValue(networkName),
            "meraki.serial"           : emit.sanitizePropertyValue(serial)
        ]

        if (addDeviceApiKey) deviceProps.put("meraki.api.key", key)

        if (networksWhitelist != null) deviceProps.put("meraki.api.org.networks", emit.sanitizePropertyValue(networksWhitelist))
        if (disableSwitches) deviceProps.put("meraki.disableswitches", "true")

        if (merakiSnmpCommunity) deviceProps.put("meraki.snmp.community.pass", merakiSnmpCommunity)
        if (merakiSnmpSecurity) deviceProps.put("meraki.snmp.security", merakiSnmpSecurity)
        if (merakiSnmpAuth) deviceProps.put("meraki.snmp.auth", merakiSnmpAuth)
        if (merakiSnmpAuthToken) deviceProps.put("meraki.snmp.authToken.pass", merakiSnmpAuthToken)
        if (merakiSnmpPriv) deviceProps.put("meraki.snmp.priv", merakiSnmpPriv)
        if (merakiSnmpPrivToken) deviceProps.put("meraki.snmp.privToken.pass", merakiSnmpPrivToken)

        if (serviceUrl) deviceProps.put("meraki.service.url", serviceUrl)

        def tags = orgDevices?.find { it.serial == serial}?.tags
        if (tags && tags != "[]") deviceProps.put("meraki.tags", tags.join(","))
    
        def firmware = orgDevices.find { it.serial == serial}?.firmware
        if (firmware) deviceProps.put("meraki.firmware", firmware)

        def address = orgDevices.find { it.serial == serial}?.address
        if (address) deviceProps.put("location", address)

        if (productType == "camera") {
            deviceProps.put("system.categories", "CiscoMerakiCamera")
        } else if (productType == "switch") {
            deviceProps.put("system.categories", "CiscoMerakiSwitch")
        } else if (productType == "wireless") {
            deviceProps.put("system.categories", "CiscoMerakiWireless")
        } else if (productType == "appliance") {
            deviceProps.put("system.categories", "CiscoMerakiAppliance")
        } else if (productType == "cellularGateway") {
            deviceProps.put("system.categories", "CiscoMerakiCellularGateway")
        } else if (productType == "sensor") {
            ip = "${name}.invalid"
            if (model == "MT10") deviceProps.put("system.categories", "CiscoMerakiTemperatureSensor,NoPing")
            if (model == "MT12") deviceProps.put("system.categories", "CiscoMerakiWaterSensor,NoPing")
            if (model == "MT20") deviceProps.put("system.categories", "CiscoMerakiDoorSensor,NoPing")
            if (model == "MT40") deviceProps.put("system.categories", "CiscoMerakiPowerSensor,NoPing")        
        }

        deviceProps.put("meraki.productType", productType)

        // Set group and collector ID based on user CSV inputs if provided
        def collectorId = null
        Map resource = [:]
        if (collectorNetworkInfo) {
            collectorId = collectorNetworkInfo[networkId]["collectorId"]
            def folder      = collectorNetworkInfo[networkId]["folder"]
            resource = [
                "hostname"    : ip,
                "displayname" : name,
                "hostProps"   : deviceProps,
                "groupName"   : ["${rootFolder}${folder}/${networkName}"],
                "collectorId" : collectorId
            ]
        } else {
            resource = [
                "hostname"    : ip,
                "displayname" : name,
                "hostProps"   : deviceProps,
                "groupName"   : ["${rootFolder}${orgDisplayname}/${networkName}"]
            ]
        }

        // Only add the collectorId field to resource map if we found a collector ID above
        if (collectorId) {
            resource["collectorId"] = collectorId
            duplicateResources["resources"][displayName]["Netscan"][0]["collectorId"] = collectorId
        }

        if (!deviceMatch) {
            resources.add(resource)
        }
    }
}

lmDebug.LMDebugPrint("Duplicate Resources:")
duplicateResources.resources.each {
    lmDebug.LMDebugPrint("\t${it}")
}

emit.resource(resources, debug)

return 0

/**
 * Processes a CSV with headers collector id, network organization device name, folder, and network
 * @param filename String
 * @return collectorInfo Map with network id as key and Map of additional attributes as value
*/
Map processCollectorNetworkInfoCSV(String filename) {
    // Read file into memory and split into list of lists
    def csv = newFile(filename, "csv")
    def rows = csv.readLines()*.split(",")
    def collectorInfo = [:]

    // Verify whether headers are present and expected values
    // Sanitize for casing and extra whitespaces while gathering headers
    def maybeHeaders = rows[0]*.toLowerCase()*.trim()
    if (maybeHeaders.contains("collector id") && maybeHeaders.contains("folder") && maybeHeaders.contains("network")) {
        Map headerIndices = [:]
        maybeHeaders.eachWithIndex{ val, i ->
            headerIndices[val] = i
        }
        // Index values for headers to ensure we key the correct index regardless of order
        def ni = headerIndices["network"]
        def ci = headerIndices["collector id"]
        def fi = headerIndices["folder"]

        // Remove headers from dataset
        def data = rows[1..-1]
        // Build a map indexed by network for easy lookups later
        data.each{ entry ->
            collectorInfo[entry[ni]] = [
                    "collectorId" : entry[ci],
                    "folder"      : entry[fi]
                ]
        }
    }
    // Bail out early if we don't have the expected headers in the provided CSV
    else {
        throw new Exception(" Required headers not provided in CSV.  Please provide \"Collector ID\", \"Network Organization Device Name\", \"Folder Name, \"and Network (case insensitive).  Headers provided: \"${rows[0]}\"")
    }

    return collectorInfo
}

/**
 * Sanitizes filepath and instantiates File object
 * @param filename String
 * @param fileExtension String
 * @return File object using sanitized relative filepath
*/
File newFile(String filename, String fileExtension) {
    // Ensure relative filepath is complete with extension type
    def filepath
    if (!filename.startsWith("./")) {
        filepath = "./${filename}"
    }
    if (!filepath.endsWith(".${fileExtension}")) {
        filepath = "${filepath}.${fileExtension}"
    }

    return new File(filepath)
}
```

10. In the **Schedule** section, select **Run this NetScan on a schedule**. For dynamic environments, you can schedule the NetScan to run as frequently as hourly.

**Note:** Subsequent NetScan runs will add or move resources or resource groups based on changes in the Cisco Meraki Dashboard. However, the NetScan cannot delete resources.

11. Select **Save** or **Save & Run**.

After running the NetScan, review the history for the number of resources added, or for error messages if the NetScan does not create any resources.

### Manually Adding Cisco Meraki Resources

^f960bf

1. Create a Cisco Meraki Organization Resource Group, or apply the following properties to an existing Resource Group: 

- Property: meraki.api.org
	- Value: Meraki Organization ID
- Property: meraki.api.key
	- Value: API key
- Property: meraki.snmp.community.pass (Applies to SNMP v1 or v2c)
	- Value: The community string

- Property: meraki.snmp.security (Applies to SNMP v3)
	- Value: Username
- Property: meraki.snmp.auth (Applies to SNMP v3)
	- Value: SHA or MD5
- Property: meraki.snmp.authToken.pass (Applies to SNMP v3)
	- Value: SHA or MD5 authentication token (password)
- Property: meraki.snmp.priv (Applies to SNMP v3)
	- Value: AES128 or DES
- Property: meraki.snmp.privToken.pass (Applies to SNMP v3)
	- Value: Privacy token (password)

**Note:** Configure the SNMP settings as Netscan Credential Properties or as Custom Properties of the Resource Group that contains your Meraki devices. Obtain the values from your Meraki Dashboard -> Organization -> Settings -> SNMP.

These settings only affect communication with snmp.meraki.com. If you are also using local SNMP DataSources, like SNMP\_Network\_Interfaces or SNMP\_Host\_Uptime, these settings will not alter their settings or operation.

2. Add the devices to the Meraki Organization Resource Group under a Network Resource Group with the following property added:
- Property: meraki.api.network
- Value: Meraki Network ID
- Property: meraki.serial
- Value: Meraki device serial
3. For more information, see the [How do I find my Org and Network ID?](https://developer.cisco.com/meraki/api/getting-started/#find-your-organization-id) documentation from Cisco Meraki.

## Import LogicModules

From the LogicMonitor Exchange, import all Cisco Meraki LogicModules from the package. For more information, see [LogicModules in Package](https://www.logicmonitor.com/support/cisco-meraki-monitoring#h-logicmodules-in-package). If these LogicModules are already installed, ensure you have the latest versions.

After the LogicModules are imported, the `addCategory_Cisco_Meraki_Device` PropertySource must run to set the required properties to begin monitoring. Data collection automaticallys begin for all devices after an elected device has started collecting data successfully in the `Cisco_Meraki_API `DataSource. For more information on importing modules, see [LM Exchange](https://www.logicmonitor.com/support/logicmodules/about-logicmodules/lm-exchange).

## Mapping Cisco Meraki Devices to LM Envision Collectors

| Collector ID | Folder | Network |
| --- | --- | --- |
| 1 | Americas | MerakiNetworkID01 |
| 1 | Americas | MerakiNetworkID02 |
| 1 | Americas | MerakiNetworkID03 |
| 1 | Americas | MerakiNetworkID04 |
| 1 | Americas | MerakiNetworkID05 |
| 1 | Americas | MerakiNetworkID06 |
| 4 | EMEA | MerakiNetworkID07 |
| 4 | EMEA | MerakiNetworkID08 |
| 4 | EMEA | MerakiNetworkID09 |
| 4 | EMEA | MerakiNetworkID10 |
| 4 | EMEA | MerakiNetworkID11 |
| 4 | EMEA | MerakiNetworkID12 |
| 7 | APAC | MerakiNetworkID13 |
| 7 | APAC | MerakiNetworkID14 |
| 7 | APAC | MerakiNetworkID15 |
| 7 | APAC | MerakiNetworkID16 |

The following are the comma-separated values for Collector ID, Folder, and Network:

```
collector id,folder,network
1,Americas,MerakiNetworkID01
1,Americas,MerakiNetworkID02
1,Americas,MerakiNetworkID03
1,Americas,MerakiNetworkID04
1,Americas,MerakiNetworkID05
1,Americas,MerakiNetworkID06
4,EMEA,MerakiNetworkID07
4,EMEA,MerakiNetworkID08
4,EMEA,MerakiNetworkID09
4,EMEA,MerakiNetworkID10
4,EMEA,MerakiNetworkID11
4,EMEA,MerakiNetworkID12
7,APAC,MerakiNetworkID13
7,APAC,MerakiNetworkID14
7,APAC,MerakiNetworkID15
7,APAC,MerakiNetworkID16
```

## Troubleshooting

- This package relies on collector script cache to continuously retrieve and store data from the Cisco Meraki API to minimize rate limiting constraints. For more information, see [Collector Script Caching](https://www.logicmonitor.com/support/collectors/collector-configurations/collector-script-caching).  
Continuous data collection is maintained on an elected Meraki device through the `Cisco_Meraki_API` DataSource, which writes API responses to the collector script cache. addCategory\_Cisco\_Meraki\_Device PropertySource must run first to set the proper category on this device. `Cisco_Meraki_API` DataSource must be running successfully for all other modules in this package to be successful.
- During onboarding, you can run Active Discovery manually on additional PropertySources in this package after `Cisco_Meraki_API` begins collecting data to expedite monitoring and topology mapping.
- If data gaps are seen, verify `Cisco_Meraki_API` is functioning successfully and check script cache health in the LogicMonitor\_Collector\_ScriptCache DataSource.

**Note:** The API used to pull data has rate limits. Check Cisco\_Meraki\_API on any Meraki device to check if the API is unreachable or monitoring has hit the API rate limit.

## Netscan Troubleshooting

The NetScan for this suite can update existing devices in the portal to add relevant information retrieved from the Cisco API. It is also possible for the NetScan to create duplicate devices in the portal when a conflict exists where the display name is the same, but the value for system.hostname is different. To ensure devices are updated properly and duplicate devices are not created, this NetScan uses LogicMonitor’s API to query existing devices and report name conflicts discovered. This file can be accessed in the collector logs.

For more information on retrieving collector logs, see [Collector Logging – Sending Logs to LogicMonitor](https://www.logicmonitor.com/support/collectors/collector-management/collector-logging#sc-header-160).

The devices in this report are not reported to LogicMonitor as part of the NetScan output unless the NetScan has been configured with the property hostname.source. This property allows a user to resolve the name conflicts discovered by selecting the source of the hostname that is used in the NetScan output. Two possible hostname sources are determined by the following values:

- “lm” or “logicmonitor” Devices with name conflicts use the existing system.hostname in LogicMonitor to ensure a device in the portal is updated using the NetScan. This setting does not create a new device.
- “netscan” Devices with name conflicts maintain system.hostname determined in the NetScan script and update the display name reported to append – `<system.hostname>` to ensure it is unique and can be added to the portal. This option is beneficial if you do not have strict naming conventions and have multiple devices with the same display name.

**Note:** NetScans are not able to update the value of `system.hostname`. NetScans can update display name, custom properties, and group assignments.

## LogicModules in Package

LogicMonitor’s package for Cisco Meraki consists of the following LogicModules. For full coverage, import all of the following LogicModules into your LogicMonitor platform:

| Display Name | Type | Description |
| --- | --- | --- |
| Cisco\_Meraki\_Topology | TopologySource | Maps Cisco Meraki Cloud topologies. |
| Cisco\_Meraki\_LinkLayer\_Topology | TopologySource | Maps Cisco Meraki Cloud CDP and LLDP link layer topologies. |
| Meraki API | DataSource | Monitors Meraki API usage. |
| Meraki Access Point Health | DataSource | Health of Meraki-managed Access Points. |
| Meraki Access Point Performance | DataSource | Performance of Meraki-managed Access Points. |
| Meraki Access Point Radios | DataSource | Radio channel utilization of Meraki-managed Access Points. |
| Meraki Access Points SSIDs | DataSource | SSID clients and traffic of Meraki-managed Access Points. |
| Meraki Camera Health | DataSource | Health of Meraki-managed Cameras. |
| Meraki Cellular Gateway | DataSource | Health of Meraki-managed Cellular Gateways |
| Meraki Cellular Gateway Performance | DataSource | Performance of Meraki-managed Cellular Gateways |
| Meraki Interfaces | DataSource | Interface throughput and packet transmission of Meraki-managed Access Points and Security Appliances. |
| Meraki Security Appliance Health | DataSource | Health of Meraki-managed Security Appliances. |
| Meraki Security Appliance Performance | DataSource | Performance of Meraki-managed Security Appliances. |
| Meraki Security Appliance Tunnels | DataSource | Tunnel metrics for Meraki-managed Security Appliances. |
| Meraki Security Appliance Underlay | DataSource | WAN connection metrics for Meraki-managed Security Appliances. |
| Meraki Switch Health | DataSource | Health of Meraki-managed Switches. |
| Meraki Switch Interfaces | DataSource | Interface data throughput of Meraki-managed Switches. |
| Meraki Switch Performance | DataSource | Performance of Meraki-managed Switches. |
| Meraki Network Events | LogSource | Provides network event logs for Cisco Meraki devices. |
| Meraki Security Appliance Events | LogSource | Provides security event logs for Cisco Meraki MX Security Appliances. |
| Meraki Assurance Alerts | LogSource | Provides assurance alerts logs on issues reported by the Meraki Dashboard API. |
| Meraki Water Sensor | DataSource | Status and health of Meraki-managed Water Sensors. |
| Meraki Temperature Sensor | DataSource | Status and health of Meraki-managed Temperature Sensors. |
| Meraki Door Sensor | DataSource | Status and health of Meraki-managed Door Sensors. |
| Meraki Power Sensor | DataSource | Status and health of Meraki-managed Power Sensors. |
| addCategory\_Cisco\_Meraki\_Device | PropertySource | Adds the CiscoMeraki<device type> system category to devices managed by Cisco Meraki, as well as other auto-properties. Requires Cisco\_Meraki\_API DataSource (observer) in order to collect data. |
| addERI\_Cisco\_Meraki\_Device | PropertySource | Discover and add Meraki specific ERI’s for Meraki resources. |

When setting static datapoint thresholds on the various metrics tracked by this package’s DataSources, LogicMonitor follows the technology owner’s best practice KPI recommendations.

**Recommendation:** As necessary, adjust the predefined thresholds to meet the unique needs of your environment. For more information on tuning datapoint thresholds, see [Tuning Static Thresholds for Datapoints](https://www.logicmonitor.com/support/alerts/about-alerts/tuning-alert-thresholds).