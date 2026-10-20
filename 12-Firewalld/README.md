# Firewalld

## Overview

Firewalld is the dynamic firewall management service used on Red Hat Enterprise Linux. It manages network traffic using zones, services, ports, and rules backed by the Linux packet-filtering framework.

System administrators use `firewall-cmd` to configure firewall behavior at runtime and persistently.

## Learning Objectives

- Understand firewalld architecture and zones.
- Inspect the active firewall configuration.
- Add and remove firewall services and ports.
- Understand runtime versus permanent configuration.
- Configure firewall rules for specific network interfaces.
- Create and manage rich rules.
- Understand masquerading and port forwarding.
- Troubleshoot firewall connectivity issues.

## Key Topics

### 1. Firewalld Fundamentals

- Firewalld service
- Zones and trust levels
- Default and active zones
- Network interface assignments
- Runtime and permanent configurations

### 2. Service and Port Management

- Listing available services
- Allowing and removing services
- Opening and closing ports
- Reloading configuration
- Verifying firewall rules

### 3. Advanced Firewall Configuration

- Rich rules
- Source address filtering
- Masquerading
- Port forwarding
- Firewall configuration files

### 4. Troubleshooting

- Checking firewall status
- Identifying active zones
- Inspecting effective rules
- Diagnosing blocked traffic
- Understanding the relationship between firewalls, routes, and listening services

## Essential Commands

```bash
sudo firewall-cmd --state
sudo firewall-cmd --get-active-zones
sudo firewall-cmd --get-default-zone
sudo firewall-cmd --zone=public --list-all
sudo firewall-cmd --add-service=http --permanent
sudo firewall-cmd --reload
sudo firewall-cmd --query-service=http
```

## Learning Outcome

By completing this module, you should be able to inspect and configure firewalld, manage services and ports, apply persistent rules, and diagnose common firewall-related connectivity problems.

## Lab Environment

- RHEL 10 virtual machine
- Root access or appropriate sudo privileges
- Firewalld installed and running
- VM console access or another recovery method
- An isolated test network for advanced rules where possible
