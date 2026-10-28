# Lab 03: Troubleshooting Service Failures with Logs

## Objective

Practice diagnosing a controlled service failure using systemd status information and journal logs.

## Prerequisites

- RHEL VM with systemd.
- `sudo` privileges.
- Console access to the VM.

## Tasks

### 1. Inspect a known service

Choose a service that exists on your system, such as `crond` or `sshd`.

```bash
sudo systemctl status crond
sudo journalctl -u crond -b
```

Record whether the service is active and whether its logs contain warnings or errors.

### 2. Create a controlled failure

Create a temporary systemd service that intentionally exits with an error. This avoids breaking an important system service.

Create the unit:

```bash
sudo tee /etc/systemd/system/log-lab-failure.service > /dev/null <<'EOF'
[Unit]
Description=Temporary logging troubleshooting lab

[Service]
Type=oneshot
ExecStart=/usr/bin/false
EOF
```

Reload systemd's unit configuration:

```bash
sudo systemctl daemon-reload
```

Run the service:

```bash
sudo systemctl start log-lab-failure.service
```

The service is expected to fail because `/usr/bin/false` returns a nonzero exit status.

### 3. Inspect the failure

```bash
sudo systemctl status log-lab-failure.service
sudo journalctl -u log-lab-failure.service -b
sudo journalctl -u log-lab-failure.service -p err
```

Identify:

- The failed unit.
- The reported exit status.
- The relevant journal messages.
- Why the command failed.

### 4. Compare status and journal output

`systemctl status` provides a concise service summary and recent log messages. `journalctl` allows more detailed filtering and historical inspection.

Try:

```bash
sudo journalctl -u log-lab-failure.service --since "10 minutes ago"
```

### 5. Remove the temporary unit

After recording your findings:

```bash
sudo rm -f /etc/systemd/system/log-lab-failure.service
sudo systemctl daemon-reload
sudo systemctl reset-failed log-lab-failure.service
```

Confirm the unit file is gone:

```bash
systemctl status log-lab-failure.service
```

A message indicating that the unit cannot be found is expected after cleanup.

## Verification

Explain:

- Why `systemctl start` can return a failure for this test.
- How the journal helps identify the cause.
- Why using a temporary service is safer than deliberately breaking SSH, networking, or another essential service.

## Cleanup

Ensure the temporary unit has been removed and systemd has reloaded its unit configuration.

## Safety

Only create and remove the exact lab unit shown here. Do not overwrite existing service units or modify system services to manufacture errors.
