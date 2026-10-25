# Scheduling Tasks in Linux

## Overview

Linux scheduling allows administrators to automate commands and scripts to run at specific times or recurring intervals.

RHEL provides several scheduling mechanisms, including `cron`, `at`, and systemd timers.

## Core Concepts

- **Cron:** Runs recurring jobs based on a time-based schedule.
- **Crontab:** Defines scheduled commands for individual users.
- **System cron directories:** `/etc/cron.hourly`, `/etc/cron.daily`, `/etc/cron.weekly`, and `/etc/cron.monthly`.
- **System crontab:** `/etc/crontab` supports an additional user field.
- **At:** Schedules a command to run once at a specified time.
- **Systemd timers:** Schedule systemd services using timer units.
- **Environment variables:** Scheduled jobs may have a more limited environment than interactive shells.
- **Logging and troubleshooting:** Help identify jobs that did not execute as expected.

## Important Commands

```bash
crontab -l
crontab -e
crontab -r
systemctl status crond
systemctl enable --now crond
atq
atrm JOB_ID
systemctl list-timers
journalctl -u crond
```

## Learning Objectives

- Understand recurring and one-time scheduling.
- Create and inspect user cron jobs.
- Understand cron time fields.
- Configure and troubleshoot the `crond` service.
- Schedule one-time jobs using `at`.
- Inspect systemd timers.
- Redirect job output and troubleshoot execution failures.
- Apply safe scheduling and logging practices.

## Best Practices

- Test commands manually before scheduling them.
- Use absolute paths for commands and files.
- Redirect output to a log when appropriate.
- Avoid scheduling duplicate jobs unintentionally.
- Check job ownership and permissions.
- Review scheduled jobs before deleting them.
- Use systemd timers when their service integration and dependency management are useful.
