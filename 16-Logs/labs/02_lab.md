# Lab 02: Journal Persistence and Log Rotation

## Objective

Inspect journal storage, determine whether logs persist across reboots, and examine traditional log rotation policies.

## Prerequisites

- RHEL VM with systemd.
- `sudo` privileges.
- Sufficient free disk space.

## Tasks

### 1. Check journal disk usage

```bash
journalctl --disk-usage
```

Record the reported size.

### 2. Inspect journal directories

```bash
ls -ld /run/log/journal
ls -ld /var/log/journal
```

A directory may not exist on your system. That is useful information, not necessarily an error.

- `/run/log/journal` is volatile storage.
- `/var/log/journal` is used for persistent journal storage.

### 3. Inspect journald configuration

```bash
grep -E '^[[:space:]]*Storage=' /etc/systemd/journald.conf
```

Inspect the full configuration if needed:

```bash
sudo less /etc/systemd/journald.conf
```

An absent or commented-out `Storage=` setting means the default behavior applies. Check the RHEL version's documentation before assuming the effective setting.

### 4. Inspect traditional log files

```bash
sudo ls -lah /var/log
```

Check common files when present:

```bash
sudo less /var/log/messages
sudo less /var/log/secure
```

Their availability depends on installed and configured logging services.

### 5. Inspect logrotate configuration

```bash
cat /etc/logrotate.conf
ls -l /etc/logrotate.d/
```

Run a debug test:

```bash
sudo logrotate -d /etc/logrotate.conf
```

Review the output for configuration errors and learn which files and policies are considered.

### 6. Understand persistence safely

Determine whether your VM uses persistent journal storage. Do not reboot merely to test persistence unless you have saved your work and have console access.

If persistent storage needs to be configured, treat that as a separate administration task: review the effective `Storage=` setting, disk capacity, permissions, and local retention requirements before making changes.

## Verification

Explain:

- The difference between volatile and persistent journals.
- Why traditional logs and journal logs may contain different information.
- How `logrotate` differs from journald's own retention controls.
- Why debug mode is safer than forcing a rotation during initial inspection.

## Cleanup

This is an inspection-only lab. No configuration changes are required.

## Safety

Do not delete journal files or manually truncate active log files to free disk space. Use supported retention and rotation controls.
