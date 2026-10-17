# Lab 03: Storage Troubleshooting and Recovery

## Objective

Practice diagnosing storage issues and recovering from an incorrect mount configuration.

## Tasks

### 1. Inspect current storage state

```bash
lsblk -f
df -hT
findmnt
sudo blkid
```

Identify:

- The practice disk and partition.
- The filesystem type.
- The UUID.
- The current mount point.

### 2. Diagnose a missing device

Inspect a device that does not exist:

```bash
lsblk /dev/nonexistent
```

Explain why a missing-device error does not necessarily indicate a storage failure.

For a real missing disk, investigate the VM configuration and available devices before changing any settings.

### 3. Inspect `/etc/fstab`

```bash
cat /etc/fstab
sudo findmnt --verify
```

Look for incorrect UUIDs, missing mount points, invalid filesystem types, or unsupported options.

### 4. Simulate an incorrect configuration safely

Only use the dedicated practice entry you created in Lab 2.

1. Back up `/etc/fstab`.
2. Edit only your `/mnt/data` entry.
3. Temporarily replace its UUID with an intentionally invalid value.
4. Save the file.
5. Run:

```bash
sudo findmnt --verify
sudo mount -a
```

Record the error messages.

**Do not modify entries for `/`, `/boot`, `/boot/efi`, swap, or other essential filesystems. Do not reboot while the test entry is incorrect.**

### 5. Recover the configuration

Restore the correct UUID in the `/mnt/data` entry.

Validate:

```bash
sudo findmnt --verify
sudo mount -a
findmnt /mnt/data
```

If validation still fails, inspect the exact error and resolve it before finishing.

### 6. Document your findings

Record:

- The original configuration.
- The fault you introduced.
- The commands used to identify it.
- The recovery steps.
- The final verification results.

## Verification Checklist

- Inspected disks and mounted filesystems.
- Examined `/etc/fstab`.
- Identified the invalid UUID.
- Restored the correct entry.
- Validated the configuration.
- Confirmed the practice filesystem mounted successfully.

## Expected Outcome

You can diagnose and recover from a persistent-mount configuration error without disrupting essential system filesystems.
