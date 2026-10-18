# Lab 01: Create PV, VG, LV, and Filesystem

## Objective

Create a complete LVM storage layout and mount the resulting filesystem.

## Prerequisites

- RHEL VM with a dedicated, empty practice disk.
- Root access or appropriate sudo privileges.
- A VM snapshot before storage changes.

## Tasks

### 1. Identify the practice disk

```bash
lsblk -o NAME,SIZE,TYPE,FSTYPE,MOUNTPOINTS
sudo pvs
sudo vgs
sudo lvs
```

Identify your additional disk by its size and configuration. The device `/dev/sdb` below is only an example.

**Do not continue until you have verified that the target disk is safe to initialize.**

### 2. Initialize a physical volume

Replace `/dev/sdb` with your verified practice device.

```bash
sudo pvcreate /dev/sdb
sudo pvs
sudo pvdisplay /dev/sdb
```

### 3. Create a volume group

```bash
sudo vgcreate vg_lab /dev/sdb
sudo vgs
sudo vgdisplay vg_lab
```

Record the total and free capacity.

### 4. Create a logical volume

Create a 1 GiB logical volume, provided the volume group has enough free space:

```bash
sudo lvcreate -L 1G -n lv_data vg_lab
sudo lvs
lsblk
```

### 5. Create an XFS filesystem

```bash
sudo mkfs.xfs /dev/vg_lab/lv_data
```

This formats the logical volume. Verify the target before running the command.

### 6. Mount the filesystem

```bash
sudo mkdir -p /mnt/lvmdata
sudo mount /dev/vg_lab/lv_data /mnt/lvmdata
```

Verify:

```bash
findmnt /mnt/lvmdata
df -hT /mnt/lvmdata
lsblk -f
```

### 7. Test file access

```bash
sudo touch /mnt/lvmdata/lvm-test.txt
ls -l /mnt/lvmdata
```

## Verification Checklist

- Created a physical volume.
- Created `vg_lab`.
- Created `lv_data`.
- Formatted the logical volume with XFS.
- Mounted it at `/mnt/lvmdata`.
- Verified the filesystem and created a test file.

## Expected Outcome

You can build and verify an LVM storage layout from a dedicated practice disk.
