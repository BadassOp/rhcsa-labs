# Lab 01: Boot and Service Failure Diagnosis

## Objective

Inspect the Linux boot sequence, identify failed services, and investigate boot-related errors using systemd and journal logs.

## Prerequisites

- RHEL VM.
- sudo privileges.
- Basic understanding of systemd and journalctl.

## Tasks

### 1. Identify the running kernel

```bash
uname -r
uname -a
cat /proc/cmdline
```

Record the running kernel version and boot parameters.

### 2. Inspect the default systemd target

```bash
systemctl get-default
systemctl list-units --type=target
```

Identify whether the system normally boots into `multi-user.target` or `graphical.target`.

### 3. Identify failed services

```bash
systemctl --failed
```

If any units have failed, inspect them:

```bash
systemctl status SERVICE_NAME
journalctl -u SERVICE_NAME -b
```

Replace `SERVICE_NAME` with an actual failed unit.

If there are no failed services, that is a valid result. Do not deliberately break an important service just to produce a failure.

### 4. Inspect boot logs

```bash
journalctl -b
journalctl -b -p warning
journalctl -b -p err
```

Identify:

- Boot-related warnings.
- Failed services.
- Filesystem or mount errors.
- Any messages indicating hardware or driver problems.

### 5. Inspect previous boot logs

```bash
journalctl --list-boots
journalctl -b -1
```

The previous boot may not be available if journal persistence was not configured.

### 6. Inspect systemd dependencies

```bash
systemctl list-dependencies multi-user.target
systemctl list-dependencies graphical.target
```

Compare the services and targets involved in the two operating states.

## Verification

Explain:

- How systemd becomes PID 1.
- How `systemctl --failed` helps diagnose startup problems.
- Why boot logs are useful even when the system successfully starts.
- Why a warning does not automatically indicate a boot failure.

## Cleanup

No configuration changes are required.

## Safety

This lab is inspection-only. Do not change the default target or disable system services during this exercise.
