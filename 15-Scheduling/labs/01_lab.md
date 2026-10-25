# Lab 01: Recurring Jobs with Cron

## Objective

Create, verify, and troubleshoot recurring cron jobs for a regular user.

## Prerequisites

- RHEL VM with `sudo` access.
- `crond` installed.
- A working user account.

## Tasks

### 1. Check the cron service

```bash
sudo systemctl status crond
sudo systemctl enable --now crond
```

### 2. Create a test script

```bash
mkdir -p ~/scheduling-lab

cat > ~/scheduling-lab/heartbeat.sh <<'EOF'
#!/bin/bash
date '+%F %T' >> "$HOME/scheduling-lab/heartbeat.log"
EOF

chmod 700 ~/scheduling-lab/heartbeat.sh
```

Test the script:

```bash
~/scheduling-lab/heartbeat.sh
cat ~/scheduling-lab/heartbeat.log
```

### 3. Create a recurring cron job

Open your user crontab:

```bash
crontab -e
```

Add this entry, replacing `/home/USERNAME` with the actual home directory:

```cron
*/2 * * * * /home/USERNAME/scheduling-lab/heartbeat.sh
```

This schedules the script every two minutes.

### 4. Verify the job

```bash
crontab -l
```

Wait a few minutes, then inspect the log:

```bash
cat ~/scheduling-lab/heartbeat.log
```

Check the cron service logs if the file is not updated:

```bash
sudo journalctl -u crond --since "10 minutes ago"
```

### 5. Practice reading schedules

For each expression, explain when the command runs:

```cron
*/5 * * * * command
0 * * * * command
30 8 * * * command
0 9 * * 1-5 command
0 0 1 * * command
```

### 6. Remove the test job

Edit your crontab:

```bash
crontab -e
```

Remove only the heartbeat entry you added, then verify:

```bash
crontab -l
```

## Verification

Explain:

- Why cron jobs should use absolute paths.
- Why a job can work interactively but fail under cron.
- The difference between a user crontab and `/etc/crontab`.

## Cleanup

```bash
rm -rf ~/scheduling-lab
```

Run this only if the directory was created for this lab and contains no files you need.

## Safety

Do not use `crontab -r` to remove a single job. It deletes the entire current user's crontab.
