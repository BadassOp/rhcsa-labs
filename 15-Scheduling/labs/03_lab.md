# Lab 03: Inspecting Systemd Timers

## Objective

Inspect systemd timers, identify the services they activate, and troubleshoot timer scheduling.

## Prerequisites

- RHEL VM with systemd.
- `sudo` access.

## Tasks

### 1. List available timers

```bash
systemctl list-timers
systemctl list-timers --all
```

Record:

- Timer name.
- Next activation.
- Last activation, if available.
- Associated service.

### 2. Inspect a timer unit

Choose an existing timer from the output. For example, if `dnf-makecache.timer` exists:

```bash
systemctl status dnf-makecache.timer
systemctl cat dnf-makecache.timer
```

Inspect its associated service:

```bash
systemctl cat dnf-makecache.service
```

If these example units are not present, select a timer and its associated service that exist on your VM.

### 3. Understand the timer configuration

Look for directives such as:

- `OnCalendar=` — calendar-based scheduling.
- `OnBootSec=` — delay after boot.
- `OnUnitActiveSec=` — interval after the associated unit was last activated.
- `Unit=` — the unit activated by the timer, when explicitly configured.

Explain how the timer determines when its service should run.

### 4. Inspect the associated service

```bash
systemctl status dnf-makecache.service
journalctl -u dnf-makecache.service -b
```

Replace the example service with the one associated with your chosen timer.

A service may be inactive between runs without indicating a problem. Check its exit status and logs after an activation.

### 5. Check timer and service relationships

```bash
systemctl list-dependencies dnf-makecache.timer
systemctl list-dependencies dnf-makecache.service
```

Use the appropriate unit names for your selected timer.

### 6. Troubleshoot a timer that did not run

Check:

```bash
systemctl status TIMER_NAME.timer
systemctl list-timers --all
systemctl cat TIMER_NAME.timer
journalctl -u TIMER_NAME.timer -b
```

Then inspect the associated service logs.

Replace `TIMER_NAME.timer` with a real timer unit. Determine whether the timer is enabled or active, whether its schedule is correct, and whether the service itself failed.

## Verification

Explain:

- The difference between a timer unit and a service unit.
- The difference between calendar-based and monotonic timers.
- Why a timer can activate successfully while its service command still fails.

## Cleanup

This lab is inspection-only and does not require configuration changes.

## Safety

Do not enable, disable, or alter system timers just to complete this lab. Some timers maintain important system tasks.
