# Storage and Disk Management — Commands Cheat Sheet

## 1. Inspect Block Devices

```bash
lsblk
lsblk -f
lsblk -o NAME,SIZE,TYPE,FSTYPE,MOUNTPOINTS,UUID
sudo fdisk -l
sudo blkid
```

- `lsblk`: Lists block devices and their relationships.
- `lsblk -f`: Displays filesystem and UUID information.
- `fdisk -l`: Lists partition tables.
- `blkid`: Identifies filesystem types, labels, and UUIDs.

## 2. Check Filesystem Usage

```bash
df -h
df -hT
du -sh /var/log
findmnt
findmnt /mnt/data
```

- `df -hT`: Displays filesystem capacity and type.
- `du -sh`: Summarizes space used by a directory.
- `findmnt`: Displays mounted filesystems and mount relationships.

## 3. Partition Management

```bash
sudo fdisk /dev/sdb
sudo parted /dev/sdb
```

Use the actual additional disk identified with `lsblk`. Device names such as `/dev/sdb` are examples only.

Inside `fdisk`, common interactive commands include:

- `p`: Print the partition table.
- `n`: Create a partition.
- `d`: Delete a partition.
- `w`: Write changes and exit.
- `q`: Exit without saving changes.

Partition changes can destroy data. Never assume `/dev/sdb` is the correct disk without verifying it.

## 4. Create Filesystems

```bash
sudo mkfs.xfs /dev/sdb1
sudo mkfs.ext4 /dev/sdb1
```

These commands are alternatives, not commands to run consecutively on the same partition. They overwrite existing filesystem structures.

## 5. Mount and Unmount

```bash
sudo mkdir -p /mnt/data
sudo mount /dev/sdb1 /mnt/data
findmnt /mnt/data
df -hT /mnt/data
sudo umount /mnt/data
```

Unmounting requires that the filesystem is not in use. Identify open files or processes if unmounting fails.

## 6. Filesystem UUIDs and Labels

```bash
sudo blkid
lsblk -f
sudo xfs_admin -L DATA /dev/sdb1
sudo e2label /dev/sdb1 DATA
```

The label commands are filesystem-specific and should be used only on the appropriate filesystem. Use a unique label when possible.

## 7. Persistent Mounts with `/etc/fstab`

Inspect the file:

```bash
cat /etc/fstab
```

Back it up before editing:

```bash
sudo cp -a /etc/fstab /etc/fstab.backup
```

Example entry for an XFS filesystem:

```fstab
UUID=<filesystem-uuid> /mnt/data xfs defaults 0 0
```

Replace the placeholder with the actual UUID and ensure the mount point exists.

Validate the configuration:

```bash
sudo findmnt --verify
sudo mount -a
findmnt /mnt/data
```

Only run `mount -a` after reviewing the entire file. It attempts to mount filesystems configured for automatic mounting.

## 8. Recovery from Mount Configuration Errors

If `mount -a` reports an error:

1. Read the exact error.
2. Identify the faulty `/etc/fstab` entry.
3. Check the UUID, mount point, filesystem type, and options.
4. Correct the entry or restore the backup.
5. Validate again using `findmnt --verify`.

Do not reboot until you have resolved or safely addressed the configuration error.

## Safety Notes

- Verify the device before partitioning or formatting.
- Never format a disk containing data you need.
- Do not unmount root, boot, or other essential filesystems.
- Keep a recovery path when editing `/etc/fstab`.
- Use a dedicated additional virtual disk for destructive practice.
