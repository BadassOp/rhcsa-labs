# Lab 03: Swap Priority and Troubleshooting

## Objective

Understand swap priority, diagnose configuration errors, and safely remove the swap file created for practice.

## Tasks

### 1. Inspect swap information

```bash
swapon --show
free -h
cat /proc/swaps
```

Record the filename, size, usage, and priority.

### 2. Understand priority

Swap priority can be specified in `/etc/fstab` using the `pri` option.

Example:

```fstab
/swapfile none swap defaults,pri=10 0 0
```

A higher priority is preferred over a lower priority swap area. If priorities are equal, Linux may distribute swap usage across the areas.

Do not modify the priority of existing production or essential swap areas during this exercise.

### 3. Diagnose a missing swap file

Inspect a nonexistent path:

```bash
sudo swapon /nonexistent-swapfile
```

Record the error and explain why this is a path or file availability issue rather than proof of a kernel failure.

### 4. Check configuration

```bash
cat /etc/fstab
sudo findmnt --verify
swapon --show
```

Confirm that the persistent swap entry refers to the correct file and uses valid options.

### 5. Clean up the practice swap file

Only perform cleanup if `/swapfile` was created by you for this lab and is not needed for another purpose.

First remove the `/swapfile` entry you added to `/etc/fstab`, then validate the file:

```bash
sudo findmnt --verify
```

Deactivate the practice swap file:

```bash
sudo swapoff /swapfile
```

Verify it is no longer active:

```bash
swapon --show
```

Finally, remove the practice file:

```bash
sudo rm /swapfile
```

Do not run the removal command if the file existed before the lab or contains anything you need.

## Verification Checklist

- Inspected active swap and priority.
- Explained how swap priority works.
- Diagnosed a nonexistent swap path.
- Validated the configuration.
- Removed only the practice entry.
- Deactivated and removed only the practice swap file.

## Expected Outcome

You can inspect swap priority, diagnose common errors, and clean up a test swap file safely.
