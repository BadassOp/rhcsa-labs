# Lab 02: One-Time Jobs with at

## Objective

Schedule a harmless command to execute once at a future time, inspect the queue, and cancel a pending job.

## Prerequisites

- RHEL VM with `sudo` access.
- The `at` package installed.
- The `atd` service available.

## Tasks

### 1. Install and start atd

```bash
sudo dnf install at
sudo systemctl enable --now atd
sudo systemctl status atd
```

### 2. Schedule a one-time job

Create a directory for the lab:

```bash
mkdir -p ~/scheduling-lab
```

Schedule a timestamp to be written two minutes later:

```bash
echo 'date "+%F %T" >> "$HOME/scheduling-lab/at-job.log"' | at now + 2 minutes
```

The command runs once under the account that submitted the job.

### 3. Inspect the queue

```bash
atq
```

Record the job ID and scheduled execution time.

### 4. Wait and verify

After the scheduled time, check:

```bash
cat ~/scheduling-lab/at-job.log
atq
```

The executed job should no longer appear in the pending queue.

### 5. Cancel a pending job

Submit another harmless job:

```bash
echo 'date >> "$HOME/scheduling-lab/cancelled-job.log"' | at now + 10 minutes
```

Inspect the queue:

```bash
atq
```

Use the ID shown by `atq`:

```bash
atrm JOB_ID
```

Confirm that the pending job has been removed.

### 6. Compare cron and at

Explain which mechanism you would choose for:

- A script that runs every night.
- A one-time command scheduled for later today.
- A service with dependencies managed by systemd.

## Verification

Explain:

- What `atd` does.
- How `atq` differs from `crontab -l`.
- Why cancelling a queued job does not undo a job that has already executed.

## Cleanup

```bash
rm -rf ~/scheduling-lab
```

Only remove the lab directory if you created it for this exercise and do not need its contents.

## Safety

Use harmless commands only. Do not schedule shutdowns, reboots, deletions, or changes to system configuration for this exercise.
