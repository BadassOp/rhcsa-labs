# Logs and Journal Commands

## 1. View the System Journal

Display journal entries:

```bash
journalctl
```

Show recent entries:

```bash
journalctl -n 50
journalctl -f
```

`-f` follows new entries as they arrive. Press `Ctrl+C` to stop following.

## 2. Inspect the Current Boot

```bash
journalctl -b
journalctl -b -p warning
journalctl -b -p err
```

List available boots:

```bash
journalctl --list-boots
```

View messages from the previous boot, if retained:

```bash
journalctl -b -1
```

## 3. Filter by Priority

```bash
journalctl -p emerg
journalctl -p alert
journalctl -p crit
journalctl -p err
journalctl -p warning
journalctl -p info
```

Priority levels range from `0` (emergency) to `7` (debug). A priority filter such as `-p err` includes messages at that severity and more severe levels.

## 4. Filter by Time

```bash
journalctl --since "1 hour ago"
journalctl --since today
journalctl --since "2026-10-28 09:00:00"
journalctl --since "2026-10-28 09:00:00" --until "2026-10-28 10:00:00"
```

Use dates and times appropriate to the investigation.

## 5. View Logs for a Service

Examples:

```bash
sudo journalctl -u sshd
sudo journalctl -u crond
sudo journalctl -u firewalld
```

Combine filters:

```bash
sudo journalctl -u sshd -b
sudo journalctl -u sshd --since "30 minutes ago"
sudo journalctl -u sshd -p warning
```

Follow service logs live:

```bash
sudo journalctl -u sshd -f
```

## 6. Inspect Traditional Log Files

```bash
sudo ls -lah /var/log
sudo less /var/log/messages
sudo less /var/log/secure
sudo less /var/log/cron
```

The files shown are common on RHEL, but their presence depends on the installed and configured logging services. Some events may be available only in the journal.

Search for errors:

```bash
sudo grep -i "error" /var/log/messages
sudo grep -i "failed" /var/log/secure
```

## 7. Check Logging Services

```bash
systemctl status systemd-journald
systemctl status rsyslog
```

`systemd-journald` is normally integrated with systemd. `rsyslog` may need to be installed and enabled separately.

## 8. Check Journal Storage

Inspect journal disk usage:

```bash
journalctl --disk-usage
```

Check whether persistent journal storage exists:

```bash
ls -ld /var/log/journal
ls -ld /run/log/journal
```

- `/run/log/journal` is volatile and normally cleared on reboot.
- `/var/log/journal` is used for persistent journal storage.

Inspect the configuration:

```bash
grep -E '^[[:space:]]*Storage=' /etc/systemd/journald.conf
```

The effective behavior can also depend on defaults and configuration snippets.

## 9. Configure Persistent Journal Storage

If persistent storage is required and appropriate for the system, create the journal directory:

```bash
sudo mkdir -p /var/log/journal
sudo systemd-tmpfiles --create --prefix /var/log/journal
```

Review the journald configuration and ensure the effective `Storage=` setting permits persistent storage, such as `Storage=persistent`.

After validating the configuration, apply it:

```bash
sudo systemctl restart systemd-journald
```

Verify:

```bash
journalctl --disk-usage
ls -ld /var/log/journal
```

Follow local policy for ownership, permissions, retention, and disk capacity.

## 10. Understand Log Rotation

Inspect the main configuration and policy files:

```bash
cat /etc/logrotate.conf
ls -l /etc/logrotate.d/
```

Test the configuration without performing rotations:

```bash
sudo logrotate -d /etc/logrotate.conf
```

Debug mode is for inspection. Avoid forcing rotation or editing production policies without understanding the impact.

## 11. Troubleshoot a Failed Service

```bash
sudo systemctl status SERVICE_NAME
sudo journalctl -u SERVICE_NAME -b
sudo journalctl -u SERVICE_NAME -p err
```

Replace `SERVICE_NAME` with the affected unit, such as `sshd` or `crond`.

Inspect the failure time, error messages, configuration, permissions, and dependent services before making changes.

## Important

Logs may contain usernames, IP addresses, authentication details, or other sensitive information. Limit access and share log excerpts carefully.
