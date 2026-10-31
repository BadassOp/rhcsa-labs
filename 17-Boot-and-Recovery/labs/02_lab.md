# Lab 02: /etc/fstab Troubleshooting

## Objective

Understand filesystem mount configuration, identify incorrect entries, and safely recover from a controlled mount-configuration problem.

## Prerequisites

- RHEL VM with sudo privileges.
- A separate test disk or filesystem.
- VirtualBox snapshot taken before starting.
- VM console access.

**Never experiment with incorrect `/etc/fstab` entries on your primary system disk.**

## Tasks

### 1. Inspect existing filesystems

```bash
lsblk -f
blkid
findmnt
```

Identify:

- Block devices.
- Filesystem types.
- UUIDs.
- Existing mount points.

### 2. Back up `/etc/fstab`

```bash
sudo cp -a /etc/fstab /etc/fstab.backup
```

Verify the backup:

```bash
sudo ls -l /etc/fstab*
```

### 3. Inspect the configuration

```bash
cat /etc/fstab
sudo findmnt --verify --verbose
```

Understand the six fields:

| Field           | Purpose                               |
| --------------- | ------------------------------------- |
| Device          | Filesystem device, UUID, or label     |
| Mount point     | Directory where filesystem is mounted |
| Filesystem type | XFS, ext4, swap, etc.                 |
| Options         | Mount options                         |
| Dump            | Backup utility field                  |
| Pass            | Filesystem check ordering field       |

### 4. Create a controlled test filesystem

Use a separate test disk or partition that contains no important data.

If using an existing test filesystem, identify it carefully with `lsblk -f` before proceeding.

Create a mount point:

```bash
sudo mkdir -p /mnt/fstab-lab
```

Record the test filesystem's UUID and type.

### 5. Add a test entry

Back up the original file first. Add an entry using the actual UUID of your test filesystem:

```text
UUID=YOUR_TEST_UUID /mnt/fstab-lab xfs defaults,nofail 0 0
```

Replace `YOUR_TEST_UUID` and `xfs` with the real values for your test filesystem.

Do not copy the example filesystem type blindly.

### 6. Validate the configuration

```bash
sudo findmnt --verify --verbose
```

Test the mount:

```bash
sudo mount /mnt/fstab-lab
findmnt /mnt/fstab-lab
```

If the filesystem is already mounted, inspect its current state rather than forcing another mount.

### 7. Understand a missing-device error

For a safe simulation, create a separate copy of the configuration and inspect it without applying it:

```bash
sudo cp /etc/fstab /tmp/fstab-lab-copy
```

Edit the copy and change only the test UUID to a nonexistent UUID.

Inspect the file:

```bash
cat /tmp/fstab-lab-copy
```

This demonstrates the type of configuration mistake that can cause a mount failure without putting the real boot configuration at risk.

### 8. Clean up the actual test entry

Remove only the entry you added to `/etc/fstab`.

Validate again:

```bash
sudo findmnt --verify --verbose
```

Unmount the test filesystem if it is mounted:

```bash
sudo umount /mnt/fstab-lab
```

## Recovery Exercise — Conceptual

If an incorrect required mount entry prevents normal boot:

1. Access the VM console.
2. Enter the recovery environment offered by the boot process.
3. Identify the root filesystem and mount it read-write if necessary.
4. Back up the incorrect configuration.
5. Correct or comment out only the problematic entry after understanding its purpose.
6. Validate the configuration.
7. Reboot and verify normal operation.

The exact commands depend on the recovery environment and filesystem layout.

## Verification

Explain:

- Why UUIDs are generally preferable to unstable device names.
- How `nofail` changes mount-failure behavior.
- Why `findmnt --verify` should be used before rebooting.
- Why an incorrect `/etc/fstab` entry can interrupt boot.

## Cleanup

```bash
sudo rm -f /tmp/fstab-lab-copy
sudo rmdir /mnt/fstab-lab
```

Remove the mount directory only after confirming it is empty and unmounted.

Keep or remove the backup according to your own lab record requirements.

## Safety

Never run `mkfs` on a disk unless you have positively identified it as a disposable test device. Never replace your complete `/etc/fstab` with a copied example.
