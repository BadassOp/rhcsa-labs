# Lab 03: Emergency and Rescue Mode

## Objective

Understand emergency and rescue modes, inspect recovery targets, and learn how to access a system for troubleshooting when normal boot fails.

## Prerequisites

- RHEL VM.
- sudo privileges.
- VirtualBox console access.
- VM snapshot before practical recovery exercises.
- RHEL installation ISO available.

## Tasks

### 1. Inspect recovery targets

```bash
systemctl cat rescue.target
systemctl cat emergency.target
```

Identify the dependencies and services associated with each target.

### 2. Compare the targets

| Target             | Purpose                                                      |
| ------------------ | ------------------------------------------------------------ |
| `rescue.target`    | Provides a recovery environment with more services available |
| `emergency.target` | Provides a minimal recovery environment                      |

Explain why emergency mode is useful when normal system initialization cannot complete.

### 3. Inspect current target relationships

```bash
systemctl list-dependencies rescue.target
systemctl list-dependencies emergency.target
```

Observe how systemd organizes the recovery environment.

### 4. Understand temporary GRUB kernel parameters

During boot:

1. Open the GRUB menu using the VM console.
2. Select the desired kernel entry.
3. Press `e` to edit the entry temporarily.
4. Inspect the kernel command line.
5. Identify where kernel parameters are specified.

Common recovery-related parameters include:

```text
systemd.unit=rescue.target
systemd.unit=emergency.target
```

These are examples for understanding recovery boot behavior.

**Do not perform this experiment on a production system.** A recovery target can interrupt services and require administrator credentials.

### 5. Understand the recovery procedure

If a system cannot boot normally:

1. Start the VM using the console.
2. Identify the earliest visible boot failure.
3. Determine whether the failure is related to GRUB, the kernel, initramfs, filesystem mounting, or systemd.
4. Enter an appropriate recovery environment.
5. Inspect logs and configuration.
6. Make the smallest necessary correction.
7. Reboot and verify normal operation.

### 6. Inspect initramfs files

```bash
ls -lh /boot/initramfs-*.img
ls -lh /boot/vmlinuz-*
```

Compare the available kernel and initramfs versions.

On RHEL, `dracut` is commonly used to generate initramfs images.

```bash
dracut --help
```

Do not rebuild or overwrite initramfs images merely for practice.

### 7. Inspect GRUB boot entries

```bash
sudo grubby --default-kernel
sudo grubby --info=ALL
```

Identify the default kernel and available boot entries.

## Verification

Explain:

- The difference between rescue and emergency targets.
- Why GRUB kernel parameters can help with recovery.
- What initramfs does during early boot.
- Why console access is important when SSH is unavailable.
- Why temporary GRUB changes are safer for testing than permanent bootloader modifications.

## Cleanup

This lab is primarily inspection and conceptual practice. If you entered a recovery target, return to normal operation using the VM console and verify the expected default target.

## Safety

Do not deliberately corrupt GRUB, delete kernel files, damage initramfs images, or modify root filesystem settings to simulate a boot failure. Use a disposable VM snapshot for advanced recovery exercises.
