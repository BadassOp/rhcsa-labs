# Swap Management

## Overview

Swap is disk-backed storage that Linux can use to hold memory pages when needed. It may be configured as a swap partition, a swap file, or a logical volume.

In Red Hat Enterprise Linux, administrators must know how to inspect swap usage, create and activate swap space, configure it persistently, and troubleshoot swap-related issues.

## Learning Objectives

- Understand the purpose of swap space.
- Inspect memory and swap usage.
- Create and initialize a swap file.
- Configure a swap partition or logical volume.
- Enable and disable swap.
- Configure persistent swap through `/etc/fstab`.
- Understand swap priority.
- Troubleshoot common swap configuration errors.

## Key Topics

### 1. Swap Fundamentals

- Physical memory versus swap
- Swap partitions, files, and logical volumes
- Swap usage and memory pressure
- Swap priority

### 2. Swap Inspection

- `free`
- `swapon`
- `swapoff`
- `/proc/swaps`
- `/proc/meminfo`

### 3. Swap File Management

- Creating a swap file
- Setting secure permissions
- Initializing swap with `mkswap`
- Activating and deactivating swap

### 4. Persistent Configuration

- UUID-based swap entries
- `/etc/fstab`
- Validating configuration
- Testing activation

### 5. Troubleshooting

- Swap not activating
- Incorrect permissions
- Invalid `/etc/fstab` entries
- Insufficient storage space
- Understanding swap usage

## Essential Commands

```bash
free -h
swapon --show
cat /proc/swaps
sudo mkswap /swapfile
sudo swapon /swapfile
sudo swapoff /swapfile
```

## Learning Outcome

By completing this module, you should be able to inspect memory and swap usage, configure swap space, activate it, and make the configuration persistent across reboots.

## Lab Environment

- RHEL virtual machine
- Root access or appropriate sudo privileges
- Sufficient free disk space
- A VM snapshot or backup before storage changes
