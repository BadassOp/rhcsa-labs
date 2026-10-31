# Boot Troubleshooting and Recovery

## Overview

Boot troubleshooting involves diagnosing and recovering Linux systems that fail to boot normally, start services incorrectly, or enter emergency or rescue mode.

On RHEL, administrators use GRUB2, the Linux kernel, initramfs, systemd targets, logs, and recovery environments to identify and resolve boot problems.

## Core Concepts

- **Boot sequence:** Firmware, bootloader, kernel, initramfs, and systemd.
- **GRUB2:** Loads the selected kernel and boot configuration.
- **Kernel parameters:** Control aspects of kernel startup and system behavior.
- **initramfs:** Provides the early userspace environment needed during boot.
- **Systemd targets:** Define groups of services and system operating states.
- **Emergency mode:** Provides a minimal environment for recovery.
- **Rescue mode:** Provides a recovery environment with more services than emergency mode.
- **GRUB editing:** Allows temporary changes to boot parameters for troubleshooting.
- **Filesystem recovery:** Includes diagnosing mount failures and incorrect `/etc/fstab` entries.
- **Boot logs:** Help identify failures during startup.

## Important Commands

```bash
systemctl get-default
systemctl list-units --type=target
systemctl --failed
journalctl -b
journalctl -b -p err
grubby --default-kernel
grubby --info=ALL
findmnt --verify
```

## Learning Objectives

- Understand the Linux boot sequence.
- Inspect GRUB2 and kernel information.
- Understand systemd targets.
- Diagnose failed services during startup.
- Investigate incorrect filesystem mount configuration.
- Understand emergency and rescue environments.
- Use logs to identify boot failures.
- Perform recovery without unnecessarily reinstalling the operating system.

## Best Practices

- Take a VM snapshot before boot-recovery exercises.
- Keep the RHEL installation ISO available for recovery.
- Back up configuration files before editing them.
- Validate `/etc/fstab` before rebooting.
- Prefer temporary boot-parameter changes when testing recovery procedures.
- Do not delete kernel or initramfs files without understanding the consequences.
- Verify normal boot after every recovery operation.
