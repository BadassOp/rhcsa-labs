# Scheduling — Mental Model

## 1. The Main Scheduling Mechanisms

| Mechanism      | Best suited for                                                   |
| -------------- | ----------------------------------------------------------------- |
| Cron           | Recurring jobs based on calendar time                             |
| At             | One-time jobs scheduled for a future time                         |
| Systemd timers | Scheduled tasks integrated with systemd services and dependencies |

Choose the mechanism based on the job's requirements rather than using cron for everything.

## 2. How Cron Works

```text
Administrator creates a crontab
             |
             v
        crond service
             |
             v
   Schedule matches current time
             |
             v
     Command is executed
             |
             v
   Output, logs, or mail
```

The `crond` service must be available and running for conventional cron jobs to execute.

## 3. Understanding the Five Time Fields

Consider:

```cron
15 6 * * 1-5 /usr/local/bin/backup.sh
```

| Field        | Value | Meaning                |
| ------------ | ----- | ---------------------- |
| Minute       | `15`  | At minute 15           |
| Hour         | `6`   | At 06:00               |
| Day of month | `*`   | Every day of the month |
| Month        | `*`   | Every month            |
| Day of week  | `1-5` | Monday through Friday  |

This schedules the command for 06:15 on weekdays.

Cron expressions define when a command should be attempted, not whether the command will succeed.

## 4. User Crontab vs. System Crontab

A user crontab has five time fields followed by the command:

```cron
0 2 * * * /home/alice/backup.sh
```

A system crontab, such as `/etc/crontab`, adds a username field:

```cron
0 2 * * * root /usr/local/sbin/backup.sh
```

The username determines which account executes the command.

## 5. The Environment Problem

An interactive shell may have:

- A customized `PATH`.
- Environment variables.
- Aliases and shell functions.
- A different working directory.

A cron job may not inherit those settings. Therefore:

- Use absolute paths.
- Set required variables explicitly.
- Specify file paths deliberately.
- Redirect output to logs when useful.

## 6. One-Time Jobs with At

The `at` mechanism stores a command for execution once at a scheduled time.

```text
Submit command
      |
      v
  at queue
      |
      v
 Scheduled time arrives
      |
      v
 Command runs once
```

Use `atq` to inspect pending jobs and `atrm` to remove a queued job.

## 7. Systemd Timers

A systemd timer is commonly paired with a service unit:

```text
example.timer
     |
     v
Activation time or interval
     |
     v
example.service
     |
     v
Command or script executes
```

Timers can support calendar-based or monotonic scheduling and integrate with systemd logging, dependencies, and service management.

## 8. Troubleshooting Sequence

When a scheduled task fails:

1. Confirm the schedule is correct.
2. Confirm the scheduler service is running.
3. Verify the job exists under the expected user.
4. Test the command manually as that user.
5. Check absolute paths, permissions, and environment variables.
6. Inspect logs and redirected output.
7. Confirm the script's exit status and expected effects.

## Core Principle

A valid schedule only determines when a command is launched. Reliable automation also requires correct permissions, a suitable environment, working dependencies, and observable output.
