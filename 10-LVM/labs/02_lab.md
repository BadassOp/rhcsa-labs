# Lab 02: Extend a Logical Volume and Filesystem

## Objective

Extend an existing logical volume and grow its XFS filesystem.

## Tasks

### 1. Inspect the current layout

```bash
sudo pvs
sudo vgs
sudo lvs
df -hT /mnt/lvmdata
```

Record the logical volume size, filesystem size, and free space in `vg_lab`.

### 2. Check available capacity

```bash
sudo vgs vg_lab
sudo lvs /dev/vg_lab/lv_data
```

Proceed only if the volume group has at least 512 MiB free.

If there is insufficient space, either skip the extension or add a second dedicated practice disk after verifying it is empty. Do not use a system disk to create extra capacity.

### 3. Extend the logical volume

Increase the logical volume by 512 MiB and request filesystem growth in the same operation:

```bash
sudo lvextend -r -L +512M /dev/vg_lab/lv_data
```

Review any error messages carefully.

### 4. Verify the result

```bash
sudo lvs /dev/vg_lab/lv_data
sudo vgs vg_lab
df -hT /mnt/lvmdata
findmnt /mnt/lvmdata
```

Confirm that the logical volume and filesystem have grown.

### 5. Test the filesystem

```bash
sudo touch /mnt/lvmdata/after-extension.txt
ls -lh /mnt/lvmdata
```

## Optional Challenge: Add a Second Disk

If you have a second empty practice disk, such as a verified `/dev/sdc`:

```bash
sudo pvcreate /dev/sdc
sudo vgextend vg_lab /dev/sdc
sudo pvs
sudo vgs
```

Replace `/dev/sdc` with the actual verified device. Confirm that the volume group has gained capacity.

Do not run `pvcreate` on a disk that contains required data or existing storage.

## Verification Checklist

- Recorded the original LV and filesystem sizes.
- Checked free space in the volume group.
- Extended the LV by 512 MiB.
- Verified filesystem growth.
- Confirmed that the filesystem remained accessible.

## Expected Outcome

You can extend an LVM logical volume and grow an XFS filesystem without recreating the filesystem.
