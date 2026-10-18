# Lab 03: Persistent Mounting and LVM Troubleshooting

## Objective

Configure persistent mounting for an LVM filesystem and troubleshoot a controlled configuration error.

## Tasks

### 1. Inspect the LVM layout

```bash
lsblk -f
sudo pvs
sudo vgs
sudo lvs
findmnt /mnt/lvmdata
```

Confirm that `/dev/vg_lab/lv_data` is mounted and accessible.

### 2. Retrieve the filesystem UUID

```bash
sudo blkid /dev/vg_lab/lv_data
```

Copy the filesystem UUID. Do not confuse it with an LVM physical volume identifier.

### 3. Back up `/etc/fstab`

```bash
sudo cp -a /etc/fstab /etc/fstab.backup
```

Add a new entry to `/etc/fstab` using the actual UUID:

```fstab
UUID=<filesystem-uuid> /mnt/lvmdata xfs defaults 0 0
```

Replace the placeholder with the real UUID. Do not add a duplicate entry for the same filesystem.

### 4. Validate the configuration

```bash
sudo findmnt --verify
sudo mount -a
findmnt /mnt/lvmdata
df -hT /mnt/lvmdata
```

Do not reboot if validation or mounting fails.

### 5. Simulate a configuration error safely

Only edit the practice entry for `/mnt/lvmdata`.

1. Make a copy of the correct entry.
2. Temporarily change the UUID to an invalid value.
3. Save the file.
4. Run:

```bash
sudo findmnt --verify
sudo mount -a
```

Record the resulting error.

Do not modify the entries for `/`, `/boot`, `/boot/efi`, swap, or other essential filesystems.

### 6. Restore the correct configuration

Restore the correct UUID and validate again:

```bash
sudo findmnt --verify
sudo mount -a
findmnt /mnt/lvmdata
```

If the filesystem is already mounted, `mount -a` may leave the existing mount in place rather than remounting it. Verify the configuration separately and test any changes using an appropriate safe procedure.

### 7. Document your findings

Record:

- The PV, VG, and LV names.
- The filesystem UUID.
- The original mount configuration.
- The error you introduced.
- How you identified and corrected it.
- The final verification results.

## Verification Checklist

- Inspected the LVM layers.
- Retrieved the filesystem UUID.
- Configured persistent mounting.
- Validated `/etc/fstab`.
- Identified an invalid UUID.
- Restored the correct entry.
- Verified the filesystem mount.

## Expected Outcome

You can configure persistent LVM mounts and troubleshoot a common mount configuration error without disrupting essential filesystems.
