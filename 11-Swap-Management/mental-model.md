# Swap Management — Mental Model

## 1. What Is Swap?

Swap is disk-backed space that Linux can use to store memory pages that are not currently needed in physical RAM.

Swap can help a system handle memory pressure, but it is much slower than RAM and does not replace having sufficient physical memory.

## 2. Swap Types

### Swap partition

A dedicated disk partition initialized for swap.

### Swap file

A regular file configured for use as swap.

### LVM logical volume

A logical volume initialized for swap.

All three can provide swap space. The appropriate choice depends on the system layout and requirements.

## 3. The Swap Lifecycle

```text
Storage Space
     |
     v
Create Swap Area
     |
     v
Initialize with mkswap
     |
     v
Activate with swapon
     |
     v
Verify with swapon --show
     |
     v
Configure /etc/fstab
     |
     v
Persistent Activation
```

Creating swap space does not automatically activate it. Activating it does not automatically make it persistent.

## 4. Runtime vs Persistent Configuration

| Operation          | Purpose                                                                      |
| ------------------ | ---------------------------------------------------------------------------- |
| `mkswap`           | Initializes a swap area                                                      |
| `swapon`           | Activates swap                                                               |
| `swapoff`          | Deactivates swap                                                             |
| `/etc/fstab` entry | Configures swap activation through the system's mount and swap configuration |
| `swapon --show`    | Verifies active swap areas                                                   |

Keep these operations distinct when troubleshooting.

## 5. Understanding Swap Usage

Use:

```bash
free -h
swapon --show
```

Swap usage alone does not prove that a system is unhealthy. Linux may retain less-used pages in swap even when memory pressure is no longer severe.

Investigate memory availability, workload behavior, and performance before drawing conclusions.

## 6. Swap File Permissions

Swap files must meet filesystem and kernel requirements. Restrictive permissions help prevent other users from accessing potentially sensitive memory contents.

Use:

```bash
sudo chmod 600 /swapfile
```

The file's location and filesystem also matter. Follow the applicable RHEL guidance if a swap file cannot be activated.

## 7. Persistent Swap Configuration

An `/etc/fstab` entry for a swap file commonly looks like:

```fstab
/swapfile none swap defaults 0 0
```

The fields identify the swap file, the absence of a normal mount point, the swap type, options, and the remaining configuration fields.

After editing, validate the file and use `swapon -a` to activate configured swap areas.

## 8. Troubleshooting Workflow

### Swap does not activate

- Check whether the file or device exists.
- Inspect permissions.
- Confirm that `mkswap` was run successfully.
- Read the error returned by `swapon`.

### Swap does not persist after reboot

- Inspect `/etc/fstab`.
- Check the file path or device identifier.
- Run `findmnt --verify`.
- Test with `swapon -a`.

### Swap usage is high

- Check available RAM with `free -h`.
- Inspect the workload and system logs.
- Avoid assuming that swap usage alone proves a memory problem.

## 9. RHCSA Exam Thinking

For swap tasks:

1. Identify the required storage location.
2. Create or identify the swap area.
3. Initialize it using `mkswap`.
4. Activate it with `swapon`.
5. Configure persistence if required.
6. Verify with `swapon --show` and `free -h`.

**Golden rule:** Initialize, activate, persist, and verify.
