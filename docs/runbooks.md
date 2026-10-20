# IT Operations Troubleshooting Runbooks

## Purpose

These runbooks provide repeatable steps for investigating common infrastructure and monitoring issues in my personal IT Operations lab.

The general approach is to verify the issue, collect evidence, identify the cause, apply a controlled fix, and confirm recovery.

## Runbook 1: Investigate High CPU Load

### Symptoms

- A Zabbix high-load alert is active.
- The server responds slowly.
- CPU utilization or load average is elevated.

### Investigation

1. Check CPU utilization and load average:

   `uptime`

   `top`

2. If available, inspect processes interactively:

   `htop`

3. Identify processes consuming excessive CPU.
4. Check whether the load is temporary or sustained.
5. Review relevant logs and recent changes if the cause is unclear.

### Resolution and Verification

- Confirm whether the workload is expected.
- On a production system, follow the approved process before stopping or restarting a process.
- Recheck CPU utilization and load average after corrective action.
- Verify that the Zabbix alert clears.

## Runbook 2: Investigate Low Disk Space

### Symptoms

- Zabbix reports high filesystem utilization.
- The server has limited available disk space.
- Applications may fail to write files or logs.

### Investigation

1. Check filesystem capacity:

   `df -h`

2. Identify directories consuming space:

   `sudo du -xhd1 / 2>/dev/null`

3. Inspect a suspected file before taking action:

   `ls -lh /path/to/file`

4. Check whether logs, temporary files, application data, or a known test file account for the usage.

### Resolution and Verification

- Confirm ownership, purpose, and retention requirements before deleting anything.
- Use approved cleanup procedures; do not remove unfamiliar system or application files.
- Run `df -h` again after cleanup.
- Confirm that filesystem utilization has returned below the alert threshold and that the Zabbix alert has cleared.

## Runbook 3: Investigate DNS Resolution Failure

### Symptoms

- A hostname such as `google.com` cannot be resolved.
- Connectivity to an IP address may still work.
- Applications report name-resolution errors.

### Investigation

1. Check basic IP connectivity:

   `ping -c 4 8.8.8.8`

2. Test hostname resolution:

   `getent hosts google.com`

3. Review resolver configuration:

   `resolvectl status`

4. Query a DNS server directly, if permitted:

   `dig @8.8.8.8 google.com +short`

5. Compare the configured DNS server with the expected network configuration.

### Resolution and Verification

- Correct the DNS configuration if an incorrect server is confirmed.
- On a system using `systemd-resolved`, use the appropriate network-management method to restore the expected resolver settings.
- Flush the resolver cache if necessary:

  `sudo resolvectl flush-caches`

- Retest hostname resolution and the affected application.
- Confirm that the DNS-related monitoring or application error is resolved.

## Runbook 4: Investigate Zabbix Agent Connectivity

### Symptoms

- A monitored host shows an unavailable or red ZBX indicator.
- Zabbix reports a connection timeout, connection refusal, or rejected connection.

### Investigation

1. Confirm the monitored host's current IP address.
2. Check the host interface address in the Zabbix frontend.
3. On the monitored machine, confirm that the Zabbix agent service is running.
4. Check the agent log for connection errors.
5. Verify that the agent's `Server` setting permits the Zabbix server's current address.
6. If active checks are used, verify the `ServerActive` setting and configured host name.
7. Check network connectivity and firewall rules for the required agent port.

For the passive agent checks used in my lab, TCP port `10050` is the agent's listening port. The Zabbix server commonly uses TCP port `10051` for incoming connections from active agents and other Zabbix components.

### Resolution and Verification

- Correct stale IP addresses or mismatched agent configuration.
- Restart the agent service only after confirming and saving the intended configuration.
- Check service status and review the logs again.
- Verify that the host becomes available in Zabbix and that fresh monitoring data is received.

## Standard Incident Handling Checklist

1. **Identify:** Record the alert, affected host, and observed symptoms.
2. **Assess:** Determine scope, severity, and potential impact.
3. **Investigate:** Collect command output, logs, monitoring data, and relevant configuration.
4. **Diagnose:** Identify the root cause using evidence.
5. **Resolve:** Apply an appropriate, controlled corrective action.
6. **Verify:** Confirm service recovery and alert clearance.
7. **Document:** Record the timeline, root cause, fix, and preventive actions.

## Scope and Safety

These procedures were developed for a personal learning lab. Production systems may require change approval, backups, escalation, and organization-specific procedures before corrective actions are performed.

