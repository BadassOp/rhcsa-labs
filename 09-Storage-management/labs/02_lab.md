# Lab 02: Mounting and Persistent Storage

## Objective

Mount a filesystem, configure automatic mounting through `/etc/fstab`, and verify the configuration safely.

## Tasks

### 1. Identify the filesystem

```bash
lsblk -f
sudo blkid
```

Identify the UUID of the partition created in Lab 1.

### 2. Create a mount point

```bash
sudo mkdir -p /mnt/data
```

Check whether `/mnt/data` is already being used as a mount point:

```bash
findmnt /mnt/data
```

If another filesystem is mounted there, do not proceed until you understand its purpose.

### 3. Mount the filesystem

Replace `/dev/sdX1` with your verified partition.

```bash
sudo mount /dev/sdX1 /mnt/data
```

Verify:

```bash
findmnt /mnt/data
df -hT /mnt/data
lsblk -f
```

### 4. Test basic file access

```bash
sudo touch /mnt/data/storage-test.txt
ls -l /mnt/data
```

### 5. Configure persistent mounting

Back up the existing configuration:

```bash
sudo cp -a /etc/fstab /etc/fstab.backup
```

Retrieve the UUID:

```bash
sudo blkid /dev/sdX1
```

Edit `/etc/fstab`:

```bash
sudo vi /etc/fstab
```

Add a new line using the actual UUID:

```fstab
UUID=<your-filesystem-uuid> /mnt/data xfs defaults 0 0
```

Do not paste the placeholder literally. Use the UUID reported by your system.

### 6. Validate before rebooting

Because the filesystem is already mounted, unmount it before testing the persistent entry:

```bash
sudo umount /mnt/data
sudo findmnt --verify
sudo mount -a
```

If validation or mounting reports an error, correct the configuration before proceeding.

Verify the mount:

```bash
findmnt /mnt/data
df -hT /mnt/data
```

Confirm the test file is present:

```bash
ls -l /mnt/data/storage-test.txt
```

### 7. Cleanup and restoration

If you want to remove the persistent mount after the exercise:

1. Back up any files you want to keep.
2. Remove only the `/mnt/data` entry you added to `/etc/fstab`.
3. Validate the remaining file using `sudo findmnt --verify`.
4. Unmount the practice filesystem when it is no longer in use.

Do not overwrite your current `/etc/fstab` with the backup if you have made other legitimate changes since creating it.

## Verification Checklist

- Mounted the practice filesystem.
- Verified it using `findmnt` and `df`.
- Created a test file.
- Added a UUID-based `/etc/fstab` entry.
- Tested the entry using `mount -a`.
- Verified the test file remained accessible.

## Expected Outcome

You can mount a filesystem and configure a reliable persistent mount using its UUID.
