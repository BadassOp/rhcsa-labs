# LVM — Commands Cheat Sheet

## 1. Inspect LVM Storage

```bash
lsblk
lsblk -f
pvs
vgs
lvs
```

Detailed inspection:

```bash
sudo pvdisplay
sudo vgdisplay
sudo lvdisplay
```

- `pvs`: Summarizes physical volumes.
- `vgs`: Summarizes volume groups.
- `lvs`: Summarizes logical volumes.

## 2. Create a Physical Volume

```bash
sudo pvcreate /dev/sdb
```

Use the verified practice disk or partition instead of `/dev/sdb`.

Inspect it:

```bash
sudo pvs
sudo pvdisplay /dev/sdb
```

**Warning:** `pvcreate` writes LVM metadata to the selected device and may overwrite existing storage metadata. Never run it on a disk containing data you need.

## 3. Create a Volume Group

```bash
sudo vgcreate vg_data /dev/sdb
```

Inspect the volume group:

```bash
sudo vgs
sudo vgdisplay vg_data
```

## 4. Create a Logical Volume

Create a 2 GiB logical volume:

```bash
sudo lvcreate -L 2G -n lv_data vg_data
```

Alternatively, allocate a percentage of the currently available space:

```bash
sudo lvcreate -l 50%FREE -n lv_extra vg_data
```

Check the result:

```bash
sudo lvs
lsblk
```

## 5. Create a Filesystem

For XFS:

```bash
sudo mkfs.xfs /dev/vg_data/lv_data
```

For ext4:

```bash
sudo mkfs.ext4 /dev/vg_data/lv_data
```

These are alternatives. Do not format a logical volume that already contains data you need.

## 6. Mount a Logical Volume

```bash
sudo mkdir -p /mnt/lvmdata
sudo mount /dev/vg_data/lv_data /mnt/lvmdata
findmnt /mnt/lvmdata
df -hT /mnt/lvmdata
```

## 7. Extend a Logical Volume

Check available space:

```bash
sudo vgs
sudo lvs
```

Extend the logical volume by 1 GiB:

```bash
sudo lvextend -L +1G /dev/vg_data/lv_data
```

Extend the logical volume and supported filesystem together:

```bash
sudo lvextend -r -L +1G /dev/vg_data/lv_data
```

The `-r` option requests filesystem resizing along with the logical volume extension. Confirm that the filesystem and environment support the operation.

## 8. Extend a Volume Group

Prepare another verified, empty practice device:

```bash
sudo pvcreate /dev/sdc
sudo vgextend vg_data /dev/sdc
```

Verify:

```bash
sudo pvs
sudo vgs
```

Do not use `/dev/sdc` without confirming that it is the correct additional disk.

## 9. Persistent Mounts

Inspect the logical volume UUID:

```bash
sudo blkid /dev/vg_data/lv_data
```

Example `/etc/fstab` entry for XFS:

```fstab
UUID=<filesystem-uuid> /mnt/lvmdata xfs defaults 0 0
```

Back up `/etc/fstab` before editing and validate the entry:

```bash
sudo cp -a /etc/fstab /etc/fstab.backup
sudo findmnt --verify
sudo mount -a
findmnt /mnt/lvmdata
```

Use the actual filesystem UUID. Test configuration changes before rebooting.

## 10. Remove LVM Storage Carefully

A typical cleanup sequence for a disposable practice volume is:

1. Back up any required files.
2. Remove the logical volume from `/etc/fstab`.
3. Unmount the filesystem.
4. Remove the logical volume.
5. Remove the volume group if no longer needed.
6. Remove physical volume metadata only when the device is no longer needed for LVM.

Example commands:

```bash
sudo umount /mnt/lvmdata
sudo lvremove /dev/vg_data/lv_data
sudo vgremove vg_data
sudo pvremove /dev/sdb
```

Only run the applicable commands, and only after verifying that the target contains no required data. Never blindly execute the whole sequence on a real system.
