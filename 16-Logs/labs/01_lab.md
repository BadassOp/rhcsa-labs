# Lab 01: journalctl and Log Filtering

## Objective

Use `journalctl` to filter logs by boot, service, time, and priority.

## Prerequisites

- RHEL VM with systemd.
- A user account with `sudo` privileges.

## Tasks

### 1. View recent journal entries

```bash
journalctl -n 30
journalctl -f
```

Press `Ctrl+C` to stop following live logs.

### 2. Inspect the current boot

```bash
journalctl -b
journalctl -b -p warning
journalctl -b -p err
```

Compare the number and severity of messages returned.

### 3. List available boots

```bash
journalctl --list-boots
```

If previous boots are retained, inspect the previous boot:

```bash
journalctl -b -1
```

If no previous boot is available, investigate journal persistence in Lab 02.

### 4. Filter by service

```bash
sudo journalctl -u sshd
sudo journalctl -u crond
sudo journalctl -u firewalld
```

Inspect recent entries for a service:

```bash
sudo journalctl -u sshd --since "30 minutes ago"
```

### 5. Filter by time and priority

```bash
journalctl --since today
journalctl --since "1 hour ago"
journalctl -p err
journalctl -p warning
```

Remember that a priority filter includes messages at that priority and more severe levels.

### 6. Combine filters

```bash
sudo journalctl -u sshd -b -p warning
sudo journalctl -u crond --since today
```

Record the service, time range, and priority used for each query.

## Verification

Explain:

- What `-b` does.
- How `-u` filters service logs.
- Why `journalctl -p err` may show more than just error messages.
- Why `journalctl -b -1` may not work on every system.

## Cleanup

No configuration changes are required.

## Safety

Logs can contain sensitive information. Avoid sharing complete logs publicly without reviewing them first.
