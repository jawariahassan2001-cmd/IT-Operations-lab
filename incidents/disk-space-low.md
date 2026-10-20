# Disk Space Incident

## Overview

I simulated a low-disk-space incident on my Ubuntu Server virtual machine to practise monitoring, filesystem investigation, and incident resolution.

I created a large test file to increase disk usage, observed the Zabbix alert, investigated the cause using Linux commands, and removed the test file to restore available space.

**Environment:** Ubuntu Server 24.04.5 LTS, Zabbix 7.0.31.

**Incident type:** Simulated lab incident.

## 1. Alert and Symptoms

Zabbix generated the following alert:

`Linux: FS [/]: Space is critically low (used > 90%, total 18.3GB)`

The alert indicated that usage on the root (`/`) filesystem had exceeded the configured 90% threshold.

Before the test, the root filesystem was approximately 34% full.

## 2. Investigation and Evidence

To simulate the issue, I created a 10 GB test file in my home directory:

`fallocate -l 10G ~/bigfile.img`

I used these commands to investigate the filesystem:

- `df -h` — checked filesystem capacity and available space.
- `du -sh /home` — checked the total space used by the home directory.
- `ls -lh ~/bigfile.img` — checked the test file's size.

The investigation showed:

- Root filesystem usage had increased to approximately 92%.
- The filesystem had approximately 1.6 GB available.
- The `/home` directory accounted for approximately 11 GB.
- The test file was 10 GB.

These results identified the file created for the simulation as the main cause of the increased disk usage.

## 3. Root Cause

The root filesystem became critically full because I deliberately created a large test file using `fallocate`.

This was a controlled lab exercise, not an unexpected production incident.

## 4. Corrective Action

After confirming the file was created for the test and was safe to remove, I deleted it:

`rm ~/bigfile.img`

I then ran `df -h` again to verify filesystem usage.

## 5. Verification and Recovery

After removing the test file:

- Root filesystem usage returned to approximately 34%.
- Available disk space recovered.
- Zabbix marked the alert as resolved at approximately 18:01:37 UTC on October 6, 2026.

The alert had been raised at approximately 17:57:37 UTC.

## 6. Prevention and Recommendations

- Monitor filesystem utilization and configure appropriate alert thresholds.
- Use `df -h` to identify filesystems with limited free space.
- Use `du` to investigate which directories consume the most space.
- Inspect large files before deleting them.
- Avoid deleting files on production systems without confirming their purpose and checking operational impact.

## 7. Lessons Learned

- Zabbix can detect filesystem usage that exceeds a configured threshold.
- `df` reports filesystem-level usage, while `du` helps investigate directory-level disk consumption.
- Large files can rapidly reduce available disk space.
- After cleanup, verify both the filesystem usage and the monitoring alert status.

## Scope

This was a simulated incident in my personal IT Operations learning lab. No production services or customers were affected.

