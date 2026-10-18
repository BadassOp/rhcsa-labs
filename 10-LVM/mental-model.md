# LVM — Mental Model

## 1. Understand the LVM Architecture

```text
Physical Disk or Partition
           |
           v
    Physical Volume (PV)
           |
           v
     Volume Group (VG)
           |
           v
    Logical Volume (LV)
           |
           v
       Filesystem
           |
           v
       Mount Point
```

### Physical Volume (PV)

A disk or partition initialized for LVM use.

Examples: `/dev/sdb` or `/dev/sdb1`.

### Volume Group (VG)

A storage pool made from one or more physical volumes.

Example: `vg_data`.

### Logical Volume (LV)

A virtual block device allocated from a volume group's free capacity.

Example: `/dev/vg_data/lv_data`.

### Filesystem

A structure such as XFS or ext4 created on the logical volume to store files and directories.

## 2. Why Use LVM?

Traditional partitions have fixed boundaries. Extending them can require additional partitioning work.

LVM separates the storage pool from the logical volumes allocated out of it. This makes it easier to allocate more capacity, add physical storage, and manage multiple volumes.

LVM does not eliminate the need for backups or careful capacity planning.

## 3. Physical Extents and Free Space

LVM divides physical volumes into units called **physical extents**. Logical volumes are allocated from the available extents in their volume group.

Inspect free capacity:

```bash
sudo pvs
sudo vgs
sudo lvs
```

A volume group can have free capacity even when a particular logical volume is full.

If the volume group has no free space, you may need to extend the group with additional storage before extending a logical volume.

## 4. Logical Volume vs Filesystem Size

These are separate layers:

- The logical volume provides block-device capacity.
- The filesystem organizes data inside that capacity.

Extending an LV does not always mean the filesystem has grown automatically.

For supported filesystems, `lvextend -r` can request both operations together. Always verify the final size with `lvs` and `df -hT`.

## 5. XFS vs ext4 When Resizing

- **XFS:** Can be grown while mounted, but cannot be shrunk in place.
- **ext4:** Can be grown and can be shrunk under the correct conditions, generally requiring it to be unmounted for shrinking and checked beforehand.

Shrinking storage is riskier than extending it. Do not practice shrinking on a volume containing important data.

## 6. Persistent Mounts

An LVM logical volume still needs to be mounted to expose its filesystem through the directory tree.

For persistent mounting:

1. Create the filesystem.
2. Create the mount point.
3. Mount the logical volume.
4. Retrieve its filesystem UUID.
5. Configure `/etc/fstab`.
6. Validate the configuration.
7. Verify the mount.

## 7. Troubleshooting Workflow

### Logical volume cannot be extended

- Inspect free capacity with `vgs`.
- Check the requested size.
- Confirm the correct volume group and logical volume.

### Volume group has insufficient space

- Inspect available physical volumes.
- Add a verified practice disk or partition if appropriate.
- Extend the volume group.

### Filesystem size has not changed

- Check the logical volume size using `lvs`.
- Check the filesystem size using `df -hT`.
- Resize the filesystem if required and supported.

### Mount fails after reboot

- Inspect `/etc/fstab`.
- Verify the filesystem UUID and type.
- Check LVM activation and journal messages.

## 8. RHCSA Exam Thinking

For an LVM task:

1. Identify the correct practice device.
2. Create a PV.
3. Create a VG.
4. Allocate an LV.
5. Create the requested filesystem.
6. Mount the filesystem.
7. Configure persistence if required.
8. Verify storage and filesystem sizes.

**Golden rule:** PV supplies storage, VG pools storage, LV allocates storage, and the filesystem stores files.
