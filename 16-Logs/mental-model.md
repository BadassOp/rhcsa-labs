# Logs and Journal — Mental Model

## 1. How Linux Logging Works

```text
Kernel, services, and applications
                 |
                 v
        Logging mechanisms
                 |
        +--------+---------+
        |                  |
        v                  v
 systemd-journald       rsyslog
        |                  |
        v                  v
 system journal       Traditional log files
                      under /var/log
```

This is a simplified model. The journal can collect messages from multiple sources, and rsyslog may receive messages from the journal or other inputs depending on configuration.

## 2. Journal vs. Traditional Log Files

| Feature     | systemd journal                                         | Traditional logs                      |
| ----------- | ------------------------------------------------------- | ------------------------------------- |
| Main tool   | `journalctl`                                            | `less`, `grep`, `tail`                |
| Filtering   | Unit, boot, time, priority                              | Text searches and file-specific tools |
| Storage     | Binary journal files                                    | Usually text files                    |
| Location    | Commonly under `/run/log/journal` or `/var/log/journal` | Commonly under `/var/log`             |
| Persistence | Depends on journal configuration and storage            | Depends on file storage and rotation  |

Both sources can be useful when diagnosing system problems.

## 3. Journal Persistence

The journal may be stored in volatile or persistent storage.

- **Volatile:** Journal files are kept under `/run` and normally do not survive a reboot.
- **Persistent:** Journal files are kept under `/var/log/journal` and can survive a reboot.

Persistence also depends on effective journald configuration, permissions, and available storage. Creating the directory alone should not replace verification.

## 4. Reading Journal Priorities

| Priority | Meaning       |
| -------- | ------------- |
| `0`      | Emergency     |
| `1`      | Alert         |
| `2`      | Critical      |
| `3`      | Error         |
| `4`      | Warning       |
| `5`      | Notice        |
| `6`      | Informational |
| `7`      | Debug         |

For example:

```bash
journalctl -p err
```

This shows error messages and more severe priorities.

## 5. How Log Rotation Works

Log rotation prevents traditional log files from growing without limit.

A rotation policy may:

1. Rename the current log.
2. Create a new log file.
3. Compress older logs.
4. Delete logs beyond the retention limit.
5. Notify or coordinate with applications when necessary.

`logrotate` commonly manages traditional logs. Journald has its own retention and disk-usage controls.

## 6. Troubleshooting Workflow

When a service fails:

1. Check `systemctl status SERVICE_NAME`.
2. Identify when the failure occurred.
3. Inspect `journalctl -u SERVICE_NAME -b`.
4. Filter by severity or time to narrow the search.
5. Check configuration, permissions, dependencies, and available disk space.
6. Apply the smallest appropriate fix.
7. Restart or reload only when necessary, then verify the result.

## 7. Common Mistakes

- Assuming every log is stored in `/var/log/messages`.
- Assuming journal entries always survive a reboot.
- Ignoring timestamps when correlating events.
- Deleting log files instead of fixing retention or disk-usage issues.
- Treating every warning as the cause of a failure.
- Sharing logs without checking for sensitive information.

## Core Principle

Logs provide evidence about what happened and when. Use targeted filters to find relevant evidence, then verify the underlying cause before changing the system.
