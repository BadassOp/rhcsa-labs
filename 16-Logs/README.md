# Logs and Journal

## Overview

Linux logging records system events, service activity, authentication attempts, warnings, and errors. On RHEL, administrators commonly use `systemd-journald`, traditional log files under `/var/log`, and `rsyslog` where installed and configured.

Logs are essential for diagnosing failed services, investigating system issues, and monitoring system health.

## Core Concepts

- **systemd-journald:** Collects and stores system and service log messages.
- **journalctl:** Queries and filters the systemd journal.
- **Persistent journal:** Stores journal data across reboots when configured.
- **`/var/log`:** Contains traditional log files and application logs.
- **rsyslog:** Provides traditional syslog processing and log-file handling when installed and enabled.
- **Log rotation:** Rotates, compresses, and removes old log files according to policy.
- **Log priorities:** Classify messages by severity, such as errors and warnings.
- **Service troubleshooting:** Uses logs to identify failures and their causes.

## Important Commands

```bash
journalctl
journalctl -b
journalctl -p err
journalctl -u sshd
journalctl --since "1 hour ago"
systemctl status rsyslog
ls -lah /var/log
sudo logrotate -d /etc/logrotate.conf
```

## Learning Objectives

- Read and filter system logs using `journalctl`.
- Inspect logs for a particular service.
- Filter messages by time and priority.
- View traditional logs under `/var/log`.
- Understand volatile versus persistent journal storage.
- Inspect logging services and configuration.
- Understand log rotation and retention.
- Troubleshoot failed services using log evidence.

## Best Practices

- Check service status and logs before changing configuration.
- Filter logs by service, boot, time, and severity.
- Protect logs because they may contain sensitive information.
- Avoid deleting logs manually to solve storage problems.
- Understand log rotation policies before changing them.
- Use timestamps and error messages to correlate events.
- Verify whether journal persistence is configured before assuming logs survive reboot.
