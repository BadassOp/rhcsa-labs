# Systemd and Services — Mental Model

## 1. What Is Systemd?

Systemd is the system and service manager. On a normal RHEL boot, it runs as PID 1 and coordinates the startup of services and other units.

Think of systemd as a manager that maintains the desired state of the system.

## 2. Understand the Three Important States

A service has separate runtime and boot-time properties.

| Command                        | Question answered                                        |
| ------------------------------ | -------------------------------------------------------- |
| `systemctl is-active SERVICE`  | Is it active now?                                        |
| `systemctl is-enabled SERVICE` | Is it configured to start automatically?                 |
| `systemctl status SERVICE`     | What is its current state and recent status information? |

Examples:

- **Active + enabled:** Running now and configured for automatic startup.
- **Active + disabled:** Running now but not enabled for automatic startup.
- **Inactive + enabled:** Not running now but configured for automatic startup.
- **Inactive + disabled:** Not running and not enabled for automatic startup.

These states are not the same as a service being healthy or reachable.

## 3. Start vs Enable

### Start

```bash
sudo systemctl start httpd
```

Starts the service now.

### Enable

```bash
sudo systemctl enable httpd
```

Configures automatic startup during the relevant boot sequence.

### Enable and start

```bash
sudo systemctl enable --now httpd
```

Use this when both immediate operation and automatic startup are required.

## 4. Restart vs Reload

- **Restart:** Stops and starts the service. This may interrupt active connections.
- **Reload:** Requests that the service reread configuration without a full restart, when supported.
- **Daemon reload:** Makes systemd reread unit definitions after unit files change.

These are three different operations.

## 5. Unit Files

A unit file describes a systemd-managed resource, its configuration, and its relationships with other units.

Common locations include:

- `/usr/lib/systemd/system/`: Vendor-provided unit files.
- `/etc/systemd/system/`: Administrator-managed unit configuration and overrides.

Prefer supported drop-in overrides instead of editing vendor unit files directly.

After changing a unit definition, run:

```bash
sudo systemctl daemon-reload
```

Then restart or otherwise update the service if the change requires it.

## 6. Targets

Targets group units to establish system states and startup goals.

Examples include:

- `multi-user.target`: A multi-user system without a graphical desktop requirement.
- `graphical.target`: A system state that includes graphical login services when configured.
- `rescue.target`: A recovery-oriented state with limited services.

Inspect the default target with:

```bash
systemctl get-default
```

## 7. Troubleshooting Workflow

When a service fails:

1. Check its status with `systemctl status`.
2. Read its logs using `journalctl -u SERVICE`.
3. Check configuration files and permissions.
4. Inspect dependencies and unit configuration.
5. Fix the underlying cause.
6. Restart the service if appropriate.
7. Verify its state and test the application.

Do not repeatedly restart a failing service without examining the error.

## 8. RHCSA Exam Thinking

For service-management tasks:

1. Identify the exact service unit.
2. Determine the required runtime state.
3. Determine whether it must start automatically at boot.
4. Use the appropriate `systemctl` command.
5. Verify both runtime and boot-time state separately.

Always verify the required final state instead of assuming a command succeeded.
