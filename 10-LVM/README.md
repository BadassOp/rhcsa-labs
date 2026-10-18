# Logical Volume Management (LVM)

## Overview

Logical Volume Management (LVM) provides a flexible way to manage storage in Red Hat Enterprise Linux. It allows administrators to combine physical storage devices into volume groups and allocate logical volumes from the available space.

Unlike traditional fixed partitions, LVM makes it easier to extend storage and manage capacity as requirements change.

## Learning Objectives

- Understand LVM architecture and terminology.
- Create physical volumes (PVs).
- Create and manage volume groups (VGs).
- Create, inspect, and remove logical volumes (LVs).
- Create filesystems on logical volumes.
- Extend logical volumes and filesystems.
- Reduce logical volumes safely where supported.
- Configure persistent mounts for logical volumes.
- Troubleshoot common LVM problems.

## Key Topics

### 1. LVM Architecture

- Physical volumes
- Volume groups
- Logical volumes
- Physical extents and logical extents

### 2. Physical Volume Management

- Identifying suitable devices
- Creating physical volumes
- Inspecting physical volumes
- Removing physical volumes safely

### 3. Volume Group Management

- Creating volume groups
- Inspecting available capacity
- Extending volume groups
- Removing physical volumes from volume groups

### 4. Logical Volume Management

- Creating logical volumes
- Inspecting logical volumes
- Extending logical volumes
- Removing logical volumes
- Understanding allocation and free space

### 5. Filesystems and Persistence

- Creating XFS or ext4 filesystems
- Mounting logical volumes
- Extending filesystems
- Persistent mounts using UUIDs and `/etc/fstab`

### 6. Troubleshooting

- Insufficient free extents
- Missing physical volumes
- Logical volumes that are not active
- Filesystem expansion problems
- Incorrect persistent mount configuration

## Essential Commands

```bash
pvs
vgs
lvs
pvcreate
vgcreate
lvcreate
lvextend
vgextend
pvdisplay
vgdisplay
lvdisplay
```

Commands that modify storage must be used only on verified practice devices.

## Learning Outcome

By completing this module, you should be able to create an LVM storage layout, allocate logical volumes, extend storage, create filesystems, and configure persistent mounts.

## Lab Environment

- RHEL virtual machine
- Root access or appropriate sudo privileges
- One or more additional virtual disks dedicated to LVM practice
- A VM snapshot before destructive storage operations
