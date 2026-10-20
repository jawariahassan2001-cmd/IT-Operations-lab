
# Zabbix Agent Connectivity Incident

## Overview

I investigated a monitoring issue in my IT Operations home lab where Zabbix could no longer communicate with my Windows 11 laptop, registered as `JEZE`.

The investigation revealed two configuration issues related to changing DHCP addresses.

**Environment:** Personal lab using Ubuntu Server, Zabbix 7.0.31, and Windows 11.

**Incident type:** Lab connectivity incident.

## 1. Symptoms

- The `JEZE` host displayed a red ZBX availability indicator in Zabbix.
- The first error was `No route to host`.
- After updating the host's IP address, the error changed to `Connection reset by peer`.

The change in error message provided new evidence and helped narrow down the investigation.

## 2. Investigation and Evidence

### Step 1: Check the Windows laptop's IP address

I checked the laptop's current network configuration using `ipconfig`.

The laptop's address had changed from `192.168.1.13` to `192.168.1.7`.

**Finding:** Zabbix was trying to reach the laptop at its previous address.

### Step 2: Update the host interface in Zabbix

I opened the `JEZE` host configuration and updated its agent interface to the laptop's current IP address.

After this change, the error changed to:

`Connection reset by peer`

**Finding:** Updating the address exposed a second issue. The agent was reachable, but the connection was being reset.

### Step 3: Check the Windows Zabbix agent log

I reviewed the agent log at:

`C:\Program Files\Zabbix Agent\zabbix_agentd.log`

The log contained messages indicating that a connection from `192.168.1.25` was rejected because the allowed hosts setting still contained `192.168.1.14`.

The log also showed a timeout connecting to the old server address.

**Finding:** The Ubuntu VM's IP address had changed from `192.168.1.14` to `192.168.1.25`, but the Windows agent configuration still referred to the old address.

## 3. Root Cause

There were two related configuration problems:

1. The Windows laptop's DHCP address changed, leaving the Zabbix host interface configured with an outdated IP address.
2. The Ubuntu VM's DHCP address changed, but the Windows agent's `Server` and `ServerActive` settings still referred to the previous server address.

A configuration edit was also initially saved to the Documents folder instead of the agent's actual configuration directory because the editor was not running with administrator privileges.

## 4. Corrective Actions

1. Updated the `JEZE` host interface in Zabbix to the laptop's current IP address.
2. Corrected the Windows agent configuration to use the updated server address and the intended allowed-host configuration.
3. Restarted the Zabbix Agent service.
4. Checked the service status and agent log for a successful startup.
5. Verified that the host's ZBX indicator returned to green.

## 5. Verification

The agent log showed a clean service startup after the configuration change, and the host subsequently returned to green in Zabbix.

The recovery time should be recorded from the confirmed Zabbix status or latest-data timestamp.

## 6. Prevention

- Consider DHCP reservations for the lab server and Windows laptop so their addresses remain stable.
- Keep the Zabbix agent's `Server` and `ServerActive` settings aligned with the monitoring server's current address.
- After changing a configuration file, confirm that it was saved in the correct directory.
- Restart the agent after configuration changes and check the logs.
- Recheck the error message after every corrective action rather than assuming the first fix resolved the entire issue.

## 7. Lessons Learned

- `No route to host` can indicate a network path or address problem; verify the destination address and connectivity.
- `Connection reset by peer` means the connection was reset by the remote side; inspect the agent configuration and logs for the reason.
- DHCP address changes can disrupt monitoring when IP addresses are configured manually.
- A change in the error message is useful evidence and may indicate that one problem was fixed while another remains.
- Verify every change through service status, logs, and monitoring recovery.

## Scope

This incident occurred in my personal learning lab. It was not a production incident.
