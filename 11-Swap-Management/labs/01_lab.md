# Lab 01: Create, Activate, and Verify Swap

## Objective

Create a swap file, initialize it, activate it, and verify that RHEL recognizes it.

## Prerequisites

- RHEL VM with root access or appropriate sudo privileges.
- At least 512 MiB of available disk space.
- A VM snapshot if possible.

## Tasks

### 1. Inspect current memory and swap

```bash
free -h
swapon --show
cat /proc/swaps
df -h /
```

Record the current RAM, swap usage, and available disk space.

### 2. Check whether the target exists

```bash
sudo ls -l /swapfile
```

If `/swapfile` already exists, **stop and choose a different filename** for this lab. Do not overwrite or modify an existing swap file.

### 3. Create the swap file

```bash
sudo dd if=/dev/zero of=/swapfile bs=1M count=512 status=progress
```

### 4. Set secure permissions

```bash
sudo chmod 600 /swapfile
ls -l /swapfile
```

Expected permissions should restrict access to the owner.

### 5. Initialize the swap area

```bash
sudo mkswap /swapfile
```

### 6. Activate swap

```bash
sudo swapon /swapfile
```

### 7. Verify

```bash
swapon --show
free -h
cat /proc/swaps
```

Confirm that `/swapfile` appears in the active swap list.

## Verification Checklist

- Inspected current memory and swap.
- Checked that the target filename did not exist.
- Created a 512 MiB swap file.
- Set permissions to `600`.
- Initialized the file using `mkswap`.
- Activated and verified swap.

## Expected Outcome

You can create, initialize, and activate a swap file on RHEL.
