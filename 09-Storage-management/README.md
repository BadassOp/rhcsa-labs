# Storage and Disk Management

## Overview

Storage management in Red Hat Enterprise Linux involves identifying block devices, creating partitions, preparing filesystems, mounting storage, and configuring persistent mounts.

System administrators must understand how storage devices connect to the filesystem and how to make storage available safely across reboots.

## Learning Objectives

- Identify disks, partitions, and block devices.
- Inspect partition tables and filesystem types.
- Create and manage partitions.
- Create filesystems using supported tools.
- Mount and unmount filesystems.
- Configure persistent mounts through `/etc/fstab`.
- Understand UUIDs and filesystem labels.
- Troubleshoot mount and filesystem issues.

## Key Topics

### 1. Block Devices

- Disks and partitions
- Device naming
- Block device inspection
- `lsblk`, `blkid`, and `fdisk`

### 2. Partition Management

- Partition tables: GPT and MBR
- Creating and deleting partitions
- Partition type and size
- Refreshing the kernel's partition information

### 3. Filesystems

- XFS and ext4
- Creating filesystems
- Inspecting filesystem types
- Filesystem labels and UUIDs

### 4. Mounting

- Mount points
- Mounting and unmounting filesystems
- Temporary mounts
- Persistent mounts
- `/etc/fstab`

### 5. Storage Troubleshooting

- Invalid mount options
- Missing devices
- Incorrect filesystem types
- Mount failures
- Safe recovery from incorrect `/etc/fstab` entries

## Essential Commands

```bash
lsblk
lsblk -f
blkid
fdisk -l
df -hT
findmnt
mount
umount
mkfs.xfs
mkfs.ext4
mount -a
```

Use filesystem-creation commands only on the intended empty device or partition.

## Learning Outcome

By completing this module, you should be able to identify storage devices, partition additional disks, create filesystems, mount storage, configure persistent mounts, and diagnose common storage problems.

## Lab Environment

- RHEL virtual machine
- Root access or appropriate sudo privileges
- An additional virtual disk dedicated to practice
- A snapshot or backup before making changes to storage configuration
