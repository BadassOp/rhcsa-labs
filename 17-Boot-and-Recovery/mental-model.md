# Boot Troubleshooting — Mental Model

## 1. Linux Boot Sequence

```text
Firmware (BIOS/UEFI)
        |
        v
Bootloader (GRUB2)
        |
        v
Linux Kernel + initramfs
        |
        v
Root filesystem becomes available
        |
        v
systemd starts as PID 1
        |
        v
Targets and system services
        |
        v
Login prompt or graphical login
```

This is a simplified sequence. Exact details depend on firmware mode, boot configuration, storage layout, and the installed RHEL version.

## 2. What Each Stage Does

| Stage          | Responsibility                                                                       |
| -------------- | ------------------------------------------------------------------------------------ |
| Firmware       | Initializes hardware and selects a boot device                                       |
| GRUB2          | Selects a boot entry and loads the kernel and initramfs                              |
| Kernel         | Initializes hardware and core operating-system functions                             |
| initramfs      | Provides early userspace tools and drivers needed to access the real root filesystem |
| systemd        | Starts units and coordinates the system's operating state                            |
| Login services | Provide console or graphical user access                                             |

A failure at an earlier stage can prevent later stages from running.

## 3. Systemd Targets

Targets group units into an intended operating state.

- `multi-user.target`: Common server operating state.
- `graphical.target`: Operating state with graphical login.
- `rescue.target`: Recovery state for troubleshooting.
- `emergency.target`: Minimal environment for severe recovery situations.

Targets are not simply equivalent to traditional runlevels, although some compatibility mappings exist.

## 4. Why `/etc/fstab` Can Break Boot

The file `/etc/fstab` describes filesystems and their mount configuration.

An incorrect entry may reference:

- A nonexistent UUID.
- An unavailable disk.
- An incorrect filesystem type.
- An invalid mount option.
- A mount point that cannot be used as configured.

If a required mount fails, boot may be delayed or interrupted.

Safe troubleshooting sequence:

1. Access the console or recovery environment.
2. Inspect the boot error.
3. Examine `/etc/fstab`.
4. Compare entries with `lsblk -f` and available UUIDs.
5. Back up and correct the configuration.
6. Validate with `findmnt --verify`.
7. Test mounts and verify normal boot.

Do not comment out required mounts blindly; understand the purpose of each entry first.

## 5. Emergency vs. Rescue Mode

| Mode      | Purpose                                                                    |
| --------- | -------------------------------------------------------------------------- |
| Rescue    | Provides a recovery environment for diagnosing and repairing a system      |
| Emergency | Provides a minimal environment when more extensive startup is not possible |

The exact available services and mounted filesystems depend on how the recovery environment is entered and configured.

## 6. Troubleshooting by Failure Stage

```text
System does not start
        |
        v
Can GRUB load?
   |          |
  No         Yes
   |          |
Bootloader   Can kernel/initramfs
recovery     initialize?
                |
                v
        Can root filesystem mount?
                |
                v
        Can systemd start services?
                |
                v
       Inspect failed units and logs
```

Use the earliest observable failure to narrow down the cause instead of changing several components at once.

## 7. Recovery Principles

- Preserve a working kernel entry whenever possible.
- Keep console or installation-media access available.
- Back up critical configuration files.
- Prefer reversible changes.
- Inspect logs before attempting repairs.
- Verify that the system boots normally after recovery.

## Core Principle

Boot recovery is a staged diagnosis process: identify the earliest failing stage, collect evidence, make a minimal correction, and verify the entire boot sequence again.
