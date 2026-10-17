# Storage and Disk Management — Mental Model

## 1. Understand the Storage Stack

```text
Physical or Virtual Disk
          |
          v
       Partition
          |
          v
      Filesystem
          |
          v
       Mount Point
          |
          v
     Accessible Files
```

A disk provides block storage. A partition divides the disk into regions. A filesystem organizes data within a device or partition. A mount point makes that filesystem accessible within the Linux directory tree.

Not every setup uses partitions: filesystems can also be created directly on a whole device, or storage may use LVM or other layers.

## 2. Device Names

Linux exposes storage devices under `/dev`.

Examples:

- `/dev/sda`: A disk.
- `/dev/sda1`: A partition on that disk.
- `/dev/sdb`: Another disk.
- `/dev/nvme0n1`: An NVMe disk.
- `/dev/nvme0n1p1`: A partition on that NVMe disk.

Device naming can change depending on hardware and virtual-machine configuration. Always identify devices using `lsblk` and supporting information before modifying them.

## 3. Filesystem vs Partition

A partition is a region of a block device. A filesystem is the data structure that organizes files and directories.

Creating a partition does not automatically create a filesystem. Formatting a partition prepares it for filesystem use.

Common filesystem types include:

- **XFS:** Commonly used as the default filesystem in RHEL installations.
- **ext4:** A widely used Linux filesystem.

Choose the filesystem appropriate for the task and environment.

## 4. Mounting

Mounting connects a filesystem to a directory.

For example:

```bash
sudo mount /dev/sdb1 /mnt/data
```

Files on that filesystem become accessible through `/mnt/data`.

The mount point should exist before mounting. If a filesystem is already mounted there, its contents may be hidden beneath the mounted filesystem until it is unmounted.

## 5. Temporary vs Persistent Mounts

### Temporary mount

A command such as `mount /dev/sdb1 /mnt/data` configures a mount at runtime. It is not, by itself, a persistent boot configuration.

### Persistent mount

An `/etc/fstab` entry tells Linux how the filesystem should be mounted automatically under the configured conditions.

A typical entry specifies:

1. The device identifier, often a UUID.
2. The mount point.
3. The filesystem type.
4. Mount options.
5. Dump setting.
6. Filesystem check order.

UUIDs are generally more reliable than `/dev/sdX` names for persistent configuration because device enumeration can change.

## 6. Why `/etc/fstab` Matters

An invalid entry can cause mount failures during boot or make a system wait for unavailable storage.

Use this workflow:

1. Back up `/etc/fstab`.
2. Add the correct entry.
3. Verify the UUID and mount point.
4. Run `findmnt --verify`.
5. Test with `mount -a`.
6. Confirm the expected mount with `findmnt`.

Testing before rebooting is an important system administration habit.

## 7. Troubleshooting Workflow

### Device not visible

- Inspect `lsblk`.
- Check the VM's virtual disk configuration.
- Rescan or refresh device information when appropriate.

### Mount fails

- Check the device, filesystem type, and mount point.
- Inspect `blkid` and `lsblk -f`.
- Read the exact error message.

### Persistent mount fails

- Inspect `/etc/fstab`.
- Verify the UUID.
- Validate mount options and filesystem type.
- Test with `findmnt --verify` and `mount -a`.

### Disk space appears exhausted

- Use `df -hT` to inspect filesystem capacity.
- Use `du -sh` to identify large directories.
- Remember that filesystem capacity and directory usage answer different questions.

## 8. RHCSA Exam Thinking

For storage tasks:

1. Identify the correct disk.
2. Confirm whether it contains data.
3. Create the required partition or storage structure.
4. Create the requested filesystem.
5. Mount it at the correct location.
6. Configure persistence if required.
7. Verify the result with `lsblk`, `findmnt`, and `df`.

**Golden rule:** Identify first, modify second, verify last.
