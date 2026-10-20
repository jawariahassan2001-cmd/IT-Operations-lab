
# IT Operations Home Lab Architecture

## Overview

This document describes my personal IT Operations learning environment. I built the lab to practise Linux administration, infrastructure monitoring, network troubleshooting, and incident management.

## Lab Components

### 1. Virtualization Host

- Operating system: Windows 11
- Hypervisor: Oracle VirtualBox
- Purpose: Runs the Ubuntu Server virtual machine.

### 2. Monitoring Server

- Operating system: Ubuntu Server 24.04.5 LTS
- Hostname: `labserver`
- Monitoring platform: Zabbix 7.0.31
- Database: MySQL 8.0.46
- Web server: Apache
- Firewall: UFW
- Remote administration: SSH

Zabbix server, its database, and the web frontend run on the same Ubuntu virtual machine.

### 3. Monitored Windows Host

- Operating system: Windows 11
- Zabbix agent: 7.0.31
- Zabbix host name: `JEZE`

The Windows laptop is registered as a separate host in Zabbix so I can practise monitoring and troubleshooting agent connectivity.

## Monitoring and Troubleshooting

The lab supports practical exercises involving:

- CPU load and resource utilization
- Disk-space thresholds and filesystem usage
- Linux service availability
- Zabbix agent connectivity
- DNS and network connectivity
- System logs and incident investigation

## Network Configuration

The virtual machine uses bridged networking over Wi-Fi and receives its IP address through DHCP.

Because DHCP addresses can change, I encountered monitoring connectivity issues when the addresses stored in Zabbix and the Windows agent configuration became outdated.

This helped me understand why stable addressing, correct agent configuration, and post-change verification matter in a monitoring environment.

## Incident Documentation

I document incidents using the following structure:

1. Symptom
2. Evidence
3. Root cause
4. Corrective action
5. Verification
6. Prevention and lessons learned

The exercises in this repository are performed in a personal learning environment, not a production enterprise infrastructure.

## Security Note

Passwords and other authentication secrets are not included in this repository. Network addresses are documented only where needed to explain a troubleshooting scenario.
