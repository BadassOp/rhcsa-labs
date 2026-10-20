# Lab 01: Firewalld Status and Zones

## Objective

Understand the firewalld service, identify active zones, and inspect interface assignments.

## Prerequisites

- RHEL system with `firewalld` installed.
- A user account with `sudo` privileges.
- A working VM console or other recovery access.

## Tasks

### 1. Check the service

```bash
sudo systemctl status firewalld
sudo firewall-cmd --state
```

If firewalld is installed but stopped, start it:

```bash
sudo systemctl enable --now firewalld
```

### 2. Inspect zones

```bash
sudo firewall-cmd --get-zones
sudo firewall-cmd --get-default-zone
sudo firewall-cmd --get-active-zones
```

Record the default zone and active zones.

### 3. Inspect the public zone

```bash
sudo firewall-cmd --zone=public --list-all
```

Identify:

- Interfaces assigned to the zone.
- Allowed services.
- Allowed ports.
- Whether masquerading is enabled.

### 4. Check interface assignments

```bash
ip -br address
sudo firewall-cmd --get-active-zones
```

Compare the network interfaces shown by both commands.

### 5. Inspect permanent configuration

```bash
sudo firewall-cmd --permanent --get-default-zone
sudo firewall-cmd --permanent --zone=public --list-all
```

Compare permanent settings with the current runtime configuration.

## Verification

You should be able to explain:

- The difference between a zone and a service.
- Which zone is active on your network interface.
- Why runtime and permanent configurations can differ.

## Cleanup

No configuration changes are required for this lab.

## Safety Note

Do not change the default zone or reassign your active network interface during this introductory lab. Incorrect changes can interrupt connectivity.
