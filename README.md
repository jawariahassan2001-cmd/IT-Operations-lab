# IT Operations Home Lab

## Overview

This repository documents my hands-on IT Operations lab, where I practise Linux administration, infrastructure monitoring, networking, and troubleshooting.

I built a lab using VirtualBox, Ubuntu Server, and Zabbix to monitor a Linux server and a Windows 11 laptop. I simulated common incidents, investigated monitoring alerts, reviewed system logs, and documented the causes and fixes.

## Environment

- VirtualBox
- Ubuntu Server 24.04.5 LTS
- Zabbix 7.0.31
- MySQL and Apache
- Windows 11
- UFW firewall and SSH

## Skills Practised

- Linux command-line administration
- Zabbix monitoring and alert investigation
- Service and agent troubleshooting
- DNS and network troubleshooting
- Firewall configuration
- System log analysis
- CPU and disk-space monitoring
- Incident documentation and root cause analysis

## Troubleshooting Reports

I documented the following incidents from my hands-on lab exercises:

- [High Load Incident](incidents/high-load.md) — Simulated CPU load, investigated resource utilization, and verified alert recovery.
- [Disk Space Incident](incidents/disk-space-low.md) — Investigated critically low disk space using `df`, `du`, and `ls`, then verified recovery.
- [DNS Resolution Failure](incidents/dns-resolution-failure.md) — Diagnosed an incorrect DNS configuration and restored name resolution.
- [Zabbix Agent Connectivity](incidents/zabbix-agent-connectivity.md) — Investigated host reachability, stale IP addresses, and Zabbix agent configuration.
## Operations Runbooks

Practical troubleshooting procedures for common infrastructure issues:

- [IT Operations Troubleshooting Runbooks](docs/runbooks.md) — Step-by-step procedures for high CPU load, low disk space, DNS resolution failures, and Zabbix agent connectivity.

## Lab Architecture

See [Lab Architecture](docs/lab-architecture.md) for details about the lab setup.

## Purpose

This personal lab helps me develop practical IT Operations skills through hands-on troubleshooting and monitoring exercises.

**Note:** This is a personal learning environment, not a production enterprise system.
