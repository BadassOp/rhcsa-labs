# Systemd and Service Management

## Overview

Systemd is the system and service manager used by RHEL. It manages system startup, services, targets, and dependencies between units.

System administrators use `systemctl` to inspect and control services and configure which services start automatically.

## Learning Objectives

- Understand systemd and systemd units.
- Inspect service status and logs.
- Start, stop, restart, and reload services.
- Enable and disable services at boot.
- Understand the difference between runtime state and boot-time configuration.
- Inspect service unit files and dependencies.
- Troubleshoot failed services.

## Key Topics

### 1. Systemd Fundamentals

- What systemd does
- PID 1 and the service manager
- Units and unit types
- Service dependencies
- Targets and system startup

### 2. Service Management

- Checking service status
- Starting and stopping services
- Restarting and reloading services
- Enabling and disabling services
- Masking and unmasking services

### 3. Unit Inspection

- Listing units
- Inspecting unit files
- Viewing service dependencies
- Understanding service states

### 4. Troubleshooting

- Failed services
- Unit configuration errors
- Service logs
- Journal inspection
- Reloading systemd configuration

## Essential Commands

```bash
systemctl status sshd
systemctl start sshd
systemctl stop sshd
systemctl restart sshd
systemctl reload sshd
systemctl enable sshd
systemctl disable sshd
systemctl is-active sshd
systemctl is-enabled sshd
systemctl --failed
systemctl list-units --type=service
```

## Learning Outcome

By completing this module, you should be able to manage services, configure boot-time service behavior, inspect unit configuration, and troubleshoot common systemd issues.

## Lab Environment

- RHEL virtual machine
- Root access or appropriate sudo privileges
- A working service such as `sshd`
- A console or VM access method for recovery if remote access is interrupted
