# Lab 02: Enable, Disable, and Boot-Time Behavior

## Objective

Understand the difference between starting a service and configuring it to start automatically at boot.

## Tasks

### 1. Inspect current state

If `httpd` is installed:

```bash
systemctl is-active httpd
systemctl is-enabled httpd
```

Record both results.

### 2. Enable the service

```bash
sudo systemctl enable httpd
systemctl is-enabled httpd
```

Enabling configures automatic startup; it does not necessarily start the service immediately.

### 3. Start and enable together

```bash
sudo systemctl enable --now httpd
systemctl is-active httpd
systemctl is-enabled httpd
```

Verify both states.

### 4. Disable automatic startup

```bash
sudo systemctl disable httpd
systemctl is-active httpd
systemctl is-enabled httpd
```

Observe that disabling does not necessarily stop a currently running service.

### 5. Restore the initial configuration

If `httpd` was installed specifically for this lab and is not needed, you can leave it stopped and disabled. Otherwise, restore the state recorded in Step 1.

If you change the state of an existing service, restore its original settings before finishing.

**Important:** Do not use `sshd` for this lab if doing so could interrupt your access. If `httpd` is unavailable, document the expected behavior using the commands and observations from Lab 1 instead.

## Verification Checklist

- Recorded the initial service state.
- Enabled a service.
- Used `enable --now`.
- Disabled automatic startup.
- Distinguished runtime state from boot-time configuration.
- Restored the original state where required.

## Expected Outcome

You can configure service startup behavior without confusing it with the service's current runtime state.
