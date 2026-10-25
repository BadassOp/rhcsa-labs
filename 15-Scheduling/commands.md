# Scheduling Commands

## 1. Manage the Cron Service

On RHEL, the cron daemon is called `crond`.

```bash
sudo systemctl status crond
sudo systemctl enable --now crond
```

Check whether it is running:

```bash
systemctl is-active crond
```

## 2. Manage User Crontabs

List the current user's scheduled jobs:

```bash
crontab -l
```

Edit the current user's crontab:

```bash
crontab -e
```

Remove the current user's entire crontab:

```bash
crontab -r
```

**Warning:** `crontab -r` removes all of the current user's cron entries. Inspect and back up existing entries before making changes.

To inspect another user's crontab as an administrator:

```bash
sudo crontab -u username -l
```

## 3. Understand Cron Time Fields

A user crontab entry contains five time fields followed by the command:

```text
minute hour day-of-month month day-of-week command
```

Example:

```cron
30 8 * * * /usr/local/bin/daily-report.sh
```

This runs the script every day at 08:30, using the scheduler's configured time zone.

Field ranges:

| Field        | Allowed values                        |
| ------------ | ------------------------------------- |
| Minute       | `0–59`                                |
| Hour         | `0–23`                                |
| Day of month | `1–31`                                |
| Month        | `1–12`                                |
| Day of week  | `0–7` (Sunday is commonly `0` or `7`) |

Common expressions:

```cron
*/5 * * * * command
0 * * * * command
0 0 * * * command
0 9 * * 1 command
```

These mean every five minutes, every hour, midnight daily, and 09:00 every Monday, respectively.

## 4. Create a Safe Practice Job

Create a script that writes a timestamp to a file in your home directory:

```bash
mkdir -p ~/scheduling-lab
cat > ~/scheduling-lab/heartbeat.sh <<'EOF'
#!/bin/bash
date >> "$HOME/scheduling-lab/heartbeat.log"
EOF
chmod 700 ~/scheduling-lab/heartbeat.sh
```

Test it manually:

```bash
~/scheduling-lab/heartbeat.sh
cat ~/scheduling-lab/heartbeat.log
```

Edit your crontab:

```bash
crontab -e
```

Add this entry to run every five minutes:

```cron
*/5 * * * * /home/USERNAME/scheduling-lab/heartbeat.sh
```

Replace `/home/USERNAME` with your actual home directory path. You can find it with:

```bash
echo "$HOME"
```

After saving, verify the entry:

```bash
crontab -l
```

## 5. Redirect Job Output

Example:

```cron
*/10 * * * * /usr/local/bin/report.sh >> /var/tmp/report.log 2>&1
```

- `>>` appends standard output to the log.
- `2>&1` redirects standard error to the same destination.

The scheduled user must have permission to write to the destination.

## 6. Inspect System Cron Configuration

```bash
cat /etc/crontab
ls -l /etc/cron.d/
ls -l /etc/cron.hourly/
ls -l /etc/cron.daily/
ls -l /etc/cron.weekly/
ls -l /etc/cron.monthly/
```

System crontab entries include a username field after the time fields, unlike ordinary user crontabs.

## 7. Schedule a One-Time Job with `at`

If the `at` package is installed and the `atd` service is available:

```bash
sudo dnf install at
sudo systemctl enable --now atd
```

Schedule a harmless one-time command:

```bash
echo 'date >> "$HOME/at-lab.log"' | at now + 2 minutes
```

List pending jobs:

```bash
atq
```

Remove a pending job:

```bash
atrm JOB_ID
```

Replace `JOB_ID` with the identifier shown by `atq`. One-time jobs run with the environment and permissions of the user who scheduled them; confirm how your environment handles shell expansion when preparing commands.

## 8. Inspect Systemd Timers

List timers:

```bash
systemctl list-timers
systemctl list-timers --all
```

Inspect a timer and its associated service:

```bash
systemctl status example.timer
systemctl cat example.timer
systemctl cat example.service
```

Replace the example unit names with actual units on your system.

## 9. Troubleshoot Cron Jobs

```bash
sudo journalctl -u crond -b
sudo journalctl -u crond --since "30 minutes ago"
sudo systemctl status crond
crontab -l
```

Also verify script permissions, executable paths, environment variables, output redirection, and file ownership.

## Important

Cron does not automatically load every interactive shell setting. Prefer absolute command paths and explicitly configure any required environment variables.# Scheduling Commands

## 1. Manage the Cron Service

On RHEL, the cron daemon is called `crond`.

```bash
sudo systemctl status crond
sudo systemctl enable --now crond
```

Check whether it is running:

```bash
systemctl is-active crond
```

## 2. Manage User Crontabs

List the current user's scheduled jobs:

```bash
crontab -l
```

Edit the current user's crontab:

```bash
crontab -e
```

Remove the current user's entire crontab:

```bash
crontab -r
```

**Warning:** `crontab -r` removes all of the current user's cron entries. Inspect and back up existing entries before making changes.

To inspect another user's crontab as an administrator:

```bash
sudo crontab -u username -l
```

## 3. Understand Cron Time Fields

A user crontab entry contains five time fields followed by the command:

```text
minute hour day-of-month month day-of-week command
```

Example:

```cron
30 8 * * * /usr/local/bin/daily-report.sh
```

This runs the script every day at 08:30, using the scheduler's configured time zone.

Field ranges:

| Field        | Allowed values                        |
| ------------ | ------------------------------------- |
| Minute       | `0–59`                                |
| Hour         | `0–23`                                |
| Day of month | `1–31`                                |
| Month        | `1–12`                                |
| Day of week  | `0–7` (Sunday is commonly `0` or `7`) |

Common expressions:

```cron
*/5 * * * * command
0 * * * * command
0 0 * * * command
0 9 * * 1 command
```

These mean every five minutes, every hour, midnight daily, and 09:00 every Monday, respectively.

## 4. Create a Safe Practice Job

Create a script that writes a timestamp to a file in your home directory:

```bash
mkdir -p ~/scheduling-lab
cat > ~/scheduling-lab/heartbeat.sh <<'EOF'
#!/bin/bash
date >> "$HOME/scheduling-lab/heartbeat.log"
EOF
chmod 700 ~/scheduling-lab/heartbeat.sh
```

Test it manually:

```bash
~/scheduling-lab/heartbeat.sh
cat ~/scheduling-lab/heartbeat.log
```

Edit your crontab:

```bash
crontab -e
```

Add this entry to run every five minutes:

```cron
*/5 * * * * /home/USERNAME/scheduling-lab/heartbeat.sh
```

Replace `/home/USERNAME` with your actual home directory path. You can find it with:

```bash
echo "$HOME"
```

After saving, verify the entry:

```bash
crontab -l
```

## 5. Redirect Job Output

Example:

```cron
*/10 * * * * /usr/local/bin/report.sh >> /var/tmp/report.log 2>&1
```

- `>>` appends standard output to the log.
- `2>&1` redirects standard error to the same destination.

The scheduled user must have permission to write to the destination.

## 6. Inspect System Cron Configuration

```bash
cat /etc/crontab
ls -l /etc/cron.d/
ls -l /etc/cron.hourly/
ls -l /etc/cron.daily/
ls -l /etc/cron.weekly/
ls -l /etc/cron.monthly/
```

System crontab entries include a username field after the time fields, unlike ordinary user crontabs.

## 7. Schedule a One-Time Job with `at`

If the `at` package is installed and the `atd` service is available:

```bash
sudo dnf install at
sudo systemctl enable --now atd
```

Schedule a harmless one-time command:

```bash
echo 'date >> "$HOME/at-lab.log"' | at now + 2 minutes
```

List pending jobs:

```bash
atq
```

Remove a pending job:

```bash
atrm JOB_ID
```

Replace `JOB_ID` with the identifier shown by `atq`. One-time jobs run with the environment and permissions of the user who scheduled them; confirm how your environment handles shell expansion when preparing commands.

## 8. Inspect Systemd Timers

List timers:

```bash
systemctl list-timers
systemctl list-timers --all
```

Inspect a timer and its associated service:

```bash
systemctl status example.timer
systemctl cat example.timer
systemctl cat example.service
```

Replace the example unit names with actual units on your system.

## 9. Troubleshoot Cron Jobs

```bash
sudo journalctl -u crond -b
sudo journalctl -u crond --since "30 minutes ago"
sudo systemctl status crond
crontab -l
```

Also verify script permissions, executable paths, environment variables, output redirection, and file ownership.

## Important

Cron does not automatically load every interactive shell setting. Prefer absolute command paths and explicitly configure any required environment variables.
