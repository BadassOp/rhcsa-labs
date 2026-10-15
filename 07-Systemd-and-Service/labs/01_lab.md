# Lab 01: Service Status and Control

## Objective

Inspect services and practice starting, stopping, restarting, and verifying their runtime state.

## Tasks

### 1. Inspect system services

```bash
systemctl --type=service
systemctl --failed
systemctl status sshd
```

Record the current state of `sshd`.

### 2. Check service state

```bash
systemctl is-active sshd
systemctl is-enabled sshd
```

Explain the difference between the two results.

### 3. Inspect another service

Check whether `httpd` is installed:

```bash
rpm -q httpd
```

If installed, inspect it:

```bash
systemctl status httpd
```

If it is not installed, use `sshd` for the remaining inspection tasks.

### 4. Start and verify a service

If `httpd` is installed, start it:

```bash
sudo systemctl start httpd
systemctl is-active httpd
```

If it is not installed, do not install it solely for this step; inspect and verify `sshd` instead.

### 5. Practice restart and stop

Only if you installed `httpd` for this lab and do not depend on it:

```bash
sudo systemctl restart httpd
systemctl status httpd
sudo systemctl stop httpd
systemctl is-active httpd
```

Do not stop or restart `sshd` if you rely on it for remote access.

## Verification Checklist

- Listed service units.
- Checked failed services.
- Inspected `sshd`.
- Compared active and enabled states.
- Practiced service control without interrupting essential services.

## Expected Outcome

You can inspect services, control their runtime state, and verify the results using `systemctl`.
