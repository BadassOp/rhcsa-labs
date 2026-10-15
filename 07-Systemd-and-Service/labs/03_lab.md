# Lab 03: Logs, Unit Files, and Troubleshooting

## Objective

Inspect systemd unit configuration, examine journal logs, and build a basic service troubleshooting workflow.

## Tasks

### 1. Inspect a unit file

```bash
systemctl cat sshd
systemctl show sshd -p FragmentPath
```

Identify the unit file path and inspect the service configuration.

### 2. Inspect dependencies

```bash
systemctl list-dependencies sshd
systemctl list-dependencies --reverse sshd
```

Record some of the listed relationships.

### 3. Read service logs

```bash
sudo journalctl -u sshd -b
sudo journalctl -u sshd -n 30
```

- `-b` limits results to the current boot.
- `-n 30` displays the most recent 30 entries.

### 4. Inspect failed units

```bash
systemctl --failed
```

If a unit is listed, inspect it:

```bash
systemctl status UNIT_NAME
sudo journalctl -u UNIT_NAME -b
```

Replace `UNIT_NAME` with the actual unit name. If no units have failed, record that result.

### 5. Inspect the default target

```bash
systemctl get-default
systemctl list-units --type=target
```

Identify the configured default target and explain its purpose.

### 6. Practice a safe troubleshooting workflow

Choose an existing service and document:

1. Its current state.
2. Its unit file location.
3. Its recent logs.
4. Its dependencies.
5. The next diagnostic step you would take if it failed.

Do not deliberately break an essential service or edit vendor unit files for this exercise.

## Verification Checklist

- Inspected a unit file.
- Checked service dependencies.
- Read journal logs.
- Inspected failed units.
- Identified the default target.
- Documented a troubleshooting workflow.

## Expected Outcome

You can investigate service configuration and logs systematically instead of relying on repeated restarts.
