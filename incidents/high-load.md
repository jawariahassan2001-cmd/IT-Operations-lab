# High Load Incident

## Overview

I simulated a high-load condition on my Ubuntu Server virtual machine to practise performance monitoring and incident troubleshooting.

I used `stress-ng` to generate CPU load and Zabbix to detect the condition. I then observed resource utilization and verified recovery after the test finished.

**Environment:** Ubuntu Server 24.04.5 LTS, Zabbix 7.0.31.

**Incident type:** Simulated lab incident.

## 1. Alert and Symptoms

Zabbix generated the alert:

`Linux: Load average is too high (per CPU load over 1.5 for 5m)`

The trigger monitored load average relative to the number of CPU cores over a five-minute window.

Before the test, the server was mostly idle, with CPU utilization around 6% and a load average of approximately 0.49 over one minute.

## 2. Investigation and Evidence

To simulate high load, I ran:

`stress-ng --cpu 2 --timeout 600s --metrics-brief`

During the test, I observed:

- CPU utilization reaching approximately 100%.
- One-minute load average reaching 2.08.
- Two `stress-ng-cpu` worker processes consuming CPU resources.
- Memory usage around 1 GB, with no swap usage observed.

I used `htop` to inspect the running processes and resource utilization.

## 3. Root Cause

The high load was caused by the `stress-ng` test I deliberately started with two CPU workers.

This was a controlled simulation, not an unexpected production failure.

## 4. Detection and Timeline

All times below are UTC.

- Approximately 07:47: Started the stress test.
- 07:52:09: Zabbix raised the high-load alert.
- Approximately 07:57: The stress test finished.
- 07:58:09: Zabbix marked the problem as resolved.

The alert appeared several minutes after the test began because the trigger required the load condition to persist over its evaluation window.

## 5. Corrective Action and Verification

The stress test had a configured timeout and finished automatically.

I observed the system after the process ended and confirmed that Zabbix subsequently marked the problem as resolved.

## 6. Prevention and Recommendations

- Inspect processes with `htop` or `top`, sorting by CPU utilization.
- Compare load average with the number of available CPU cores.
- Review memory and swap usage to identify other resource constraints.
- Configure alert thresholds and evaluation windows according to the monitoring requirements.
- Investigate the process and its purpose before stopping it on a real server.

## 7. Lessons Learned

- High CPU utilization and high load average are related but different measurements.
- Load average must be interpreted in relation to the number of CPU cores and the workload.
- Monitoring trigger windows help reduce short-lived alerts but can delay detection.
- After a corrective action, verify that resource utilization returns to normal and the alert resolves.

## Scope

This was a simulated incident in my personal IT Operations learning lab. No production services or customers were affected.

