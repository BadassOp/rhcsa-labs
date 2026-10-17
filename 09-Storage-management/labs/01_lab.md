# Lab 01: Disk Identification, Partitioning, and Filesystems

## Objective

Identify a block device, create a partition, and format it with a Linux filesystem.

## Prerequisites

- RHEL VM with an additional, dedicated practice disk.
- Root access or appropriate sudo privileges.
- No required data on the practice disk.

## Tasks

### 1. Identify available disks

```bash
lsblk
lsblk -f
sudo fdisk -l
```

Identify your additional disk by its size and device information.

**Do not assume the device is `/dev/sdb`.** Confirm the correct device before continuing.

### 2. Inspect the partition table

Replace `/dev/sdX` with your verified practice disk.

```bash
sudo fdisk -l /dev/sdX
```

Record:

- Disk size.
- Partition table type, if present.
- Existing partitions.

If the disk contains data or partitions you need, stop here and use a separate empty practice disk.

### 3. Create a partition

```bash
sudo fdisk /dev/sdX
```

Inside `fdisk`:

1. Enter `p` to inspect the current layout.
2. Enter `n` to create a partition.
3. Accept the default partition number and sector range to use the available space, if appropriate.
4. Enter `p` to verify the proposed layout.
5. Enter `w` to save changes.

Refresh the device view:

```bash
lsblk
```

If the kernel has not detected the new partition, investigate before proceeding.

### 4. Create an XFS filesystem

Replace `/dev/sdX1` with the actual new partition.

```bash
sudo mkfs.xfs /dev/sdX1
```

**Warning:** Formatting destroys existing filesystem structures on the target partition. Verify the device carefully.

### 5. Verify the filesystem

```bash
lsblk -f
sudo blkid /dev/sdX1
sudo fdisk -l /dev/sdX
```

Record the filesystem type and UUID.

## Verification Checklist

- Identified the additional disk.
- Inspected its partition table.
- Created a partition.
- Created an XFS filesystem.
- Verified the filesystem type and UUID.

## Expected Outcome

You can identify storage devices, create a partition, and prepare it with an XFS filesystem.
